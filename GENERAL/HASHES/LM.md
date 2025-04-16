
###  ESTRUCTURA DEL HASH

```
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
    

###  USOS EN ATAQUES

- **Crackeo instantáneo** con diccionario o rainbow tables
    
- Si está habilitado, **facilita Pass-the-Hash**
    
- Puede aparecer en respuestas NTLMv1 (`LM Response`)
    

## CRACKEO
 

```
# Hashcat
hashcat -m 3000 hashes.txt rockyou.txt

# John
john --format=lm hashes.txt --wordlist=rockyou.txt
```

---