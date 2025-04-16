---
sticker: ""
tags:
---
# 🔐 HASHES WINDOWS – GUÍA COMPLETA

---

## 🔴 LM (LAN MANAGER)

### ESTRUCTURA DEL HASH

```plaintext
aad3b435b51404eeaad3b435b51404ee
```

### CARACTERISTICAS

- Convierte la contraseña a mayúsculas
    
- Corta la contraseña en 2 bloques de 7 caracteres
    
- Cada bloque se cifra con DES
    
- Sin sal, sin aleatoriedad
    
- Muy débil y obsoleto
    

### COMO SE OBTIENE

- Desde la base de datos SAM (`C:\Windows\System32\config\SAM`)
    
- Herramientas:
    
    - `mimikatz`
        
    - `secretsdump.py`
        
    - `samdump2`
        

###  USOS LEGITIMOS

- Solo en **versiones antiguas de Windows**
    
- Compatibilidad con sistemas legacy
    

### USOS EN ATAQUES

- **Crackeo instantáneo** con diccionario o rainbow tables
    
- Si está habilitado, **facilita Pass-the-Hash**
    
- Puede aparecer en respuestas NTLMv1 (`LM Response`)
    

### CRACKEO

```bash
# Hashcat
hashcat -m 3000 hashes.txt rockyou.txt
```

```bash
# John
john --format=lm hashes.txt --wordlist=rockyou.txt
```

---

## 🟡 NT HASH (a.k.a. NTLM hash)

### ESTRUCTURA DEL HASH

```plaintext
32ed87bdb5fdc5e9cba88547376818d4
```

###  CARACTERISTICAS

- Base para NTLMv1, NTLMv2 y Kerberos
    
- Siempre igual para la misma contraseña
    
- Sin sal, vulnerable a diccionarios y rainbow tables
    

### COMO SE OBTIENE

- Volcado desde:
    
    - SAM (usuarios locales)
        
    - NTDS.dit (usuarios de dominio)
        
    - Memoria (LSASS)
        
- Herramientas:
    
    - `mimikatz`
        
    - `secretsdump.py`
        

### USOS LEGITIMOS

- Comparación local de contraseña
    
- Base para autenticación NTLM (v1/v2)
    
- Clave secreta para Kerberos
    

###  USOS EN ATAQUES

- **Pass-the-Hash**
    
- **Overpass-the-Hash (Kerberos)**
    
- Lateral movement
    
- Reutilización directa del hash sin crackeo
    

###  CRACKEO

```bash
# Hashcat
hashcat -m 1000 hashes.txt rockyou.txt
```

```bash
# John
john --format=nt hashes.txt --wordlist=rockyou.txt
```

---

## 🟠 NTLMv1

###  ESTRUCTURA DEL HASH

```plaintext
usuario::DOMINIO:challenge:LM_response:NT_response
```

###  CARACTERISTICAS

- Usa el NT hash como clave DES (3 bloques de 7 bytes)
    
- No usa sal ni blob
    
- Muy débil
    
- Fácilmente crackeable offline
    

###  COMO SE OBTIENE

- Captura por red (SMB, HTTP, LDAP sin Kerberos)
    
- Herramientas:
    
    - `Responder`
        
    - `ntlmrelayx.py`
        
    - `Wireshark`
        

### USOS LEGITIMOS

- Autenticación en red en sistemas antiguos
    
- Fallback de NTLMv2 en entornos mal configurados
    

###  USOS EN ATAQUES

- **Relay de autenticación (SMB/HTTP)**
    
- **Crackeo offline inmediato**
    
- Enumeración de privilegios
    
- Escalado sin necesidad de contraseña
    

###  CRACKEO

```bash
# Hashcat
hashcat -m 5500 hashes.txt rockyou.txt
```

```bash
# John
john --format=netntlm hashes.txt --wordlist=rockyou.txt
```

---

## 🟢 NTLMv2 (Net-NTLMv2)

###  ESTRUCTURA DEL HASH

```plaintext
usuario::DOMINIO:challenge:NTLMv2_response:blob
```

###  CARACTERISTICAS

- Mucho más seguro que NTLMv1
    
- Evita replay attacks con timestamps y aleatoriedad
    
- Aún crackeable si la contraseña es débil
    
- Muy común en redes modernas sin Kerberos
    

###  COMO SE OBTIENE

- Captura por red en autenticación NTLM (SMB, HTTP, LDAP)
    
- Herramientas:
    
    - `Responder`
        
    - `ntlmrelayx.py`
        
    - `Wireshark`
        
    - UNC phishing (`\\attacker\share`)
        

### USOS LEGITIMOS

- Autenticación en red cuando Kerberos no está disponible
    
- Fallback automático de Kerberos
    

### USOS EN ATAQUES

- **Crackeo offline sin tener el host**
    
- **Relay de autenticación**
    
- **Acceso a servicios SMB, LDAP, HTTP**
    
- Recolección silenciosa con phishing o MITM
    

###  CRACKEO

```bash
# Hashcat
hashcat -m 5600 hashes.txt rockyou.txt
```

```bash
# John
john --format=netntlmv2 hashes.txt --wordlist=rockyou.txt
```
