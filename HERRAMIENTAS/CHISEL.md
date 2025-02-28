# Chisel

Chisel es una herramienta de tunneling que se utiliza para crear túneles TCP y conexiones inversas entre sistemas. Es una herramienta ligera y eficiente que permite la comunicación entre equipos separados por un firewall o NAT, facilitando el acceso a servicios internos de una red que normalmente no estarían accesibles. Funciona en modo cliente-servidor, donde un extremo actúa como servidor y el otro como cliente, permitiendo múltiples tipos de tunelización.

## Funcionalidades de Chisel

### 1. Tunneling TCP

- Chisel permite crear túneles TCP para acceder a servicios internos de una red que no están expuestos externamente.
- **Ejemplo**: Si estamos en una red interna y queremos acceder a un servicio HTTP en el puerto 8080 de una máquina remota, podemos usar Chisel para crear un túnel desde nuestro equipo hasta esa máquina.

    ```bash
    # Comando en la máquina remota (servidor)
    chisel server -p 8080 --reverse

    # Comando en el cliente
    chisel client <IP_SERVER>:8080 R:8080:<IP_LOCAL>:8080
    ```
    Esto creará un túnel que redirige el puerto 8080 del servidor al puerto 8080 del cliente.

### 2. Reverse Tunneling

- El reverse tunneling es útil cuando se necesita acceder a un servicio del cliente desde el servidor.
- **Ejemplo**: Si queremos acceder al puerto 22 (SSH) de una máquina cliente desde el servidor, podemos usar el siguiente comando:

    ```bash
    # Comando en el cliente
    chisel client <IP_SERVER>:8080 R:2222:localhost:22

    # Desde el servidor accedemos al cliente vía SSH
    ssh -p 2222 usuario@localhost
    ```

### 3. Conexión a través de Proxy

- Chisel permite la conexión a través de proxies HTTP y SOCKS5, útil en entornos con restricciones.
- **Ejemplo**: Conectarse a través de un proxy SOCKS5:

    ```bash
    chisel client --socks5 <IP_SERVER>:8080 socks
    ```
    Esto creará un proxy SOCKS5 en el cliente para redirigir tráfico a través del servidor.

### 4. Combinación con Otras Herramientas

- **Metasploit**: Chisel se puede usar para abrir túneles a través de un sistema comprometido y redirigir el tráfico hacia la red interna.
    - **Ejemplo**: Si obtenemos acceso a una máquina con Metasploit y queremos redirigir el tráfico hacia un servicio interno, podemos ejecutar Chisel en el sistema comprometido y redirigir el tráfico hacia Metasploit para realizar escaneo de puertos internos.

    ```bash
    # En la máquina comprometida (servidor Chisel)
    chisel server -p 8080

    # En nuestro equipo (cliente Chisel)
    chisel client <IP_COMPROMETIDA>:8080 1080:socks

    # En Metasploit podemos usar el módulo auxiliary/scanner/portscan/tcp a través del túnel SOCKS5.
    ```

- **Proxychains**: Combinado con proxychains, podemos utilizar Chisel para encapsular el tráfico de otras herramientas.
    - **Ejemplo**: Redirigir el tráfico de `nmap` a través de Chisel:

    ```bash
    # Configuración de proxychains
    proxychains4 nmap -sT -Pn <IP_INTERNA>

    # Configuramos proxychains para usar el puerto SOCKS5 del túnel Chisel.
    ```

- **SSH**: Podemos usar Chisel para crear túneles SSH y redirigir tráfico de manera eficiente.
    - **Ejemplo**: Si queremos usar un túnel SSH para acceder a una base de datos interna:

    ```bash
    # Creamos el túnel con Chisel
    chisel client <IP_SERVER>:8080 5432:localhost:5432

    # Desde nuestro equipo nos conectamos a la base de datos
    psql -h localhost -p 5432 -U usuario -d base_datos
    ```

### 5. Escenarios de Uso

- **Acceso a Servidores Internos**: Chisel se usa para acceder a servicios internos de una red, como bases de datos, servidores web o servicios de administración.
- **Exfiltración de Datos**: Permite transferir datos desde un sistema comprometido hacia un servidor controlado por el atacante.
- **Bypass de Firewalls**: Chisel ayuda a evadir restricciones de firewalls al encapsular tráfico en conexiones autorizadas.

## Resumen de Funcionalidades

| Funcionalidad                     | Descripción                                                                                          | Ejemplo                                                    |
|-----------------------------------|----------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| **Tunneling TCP**                 | Crear túneles TCP para acceder a servicios internos de una red.                                      | Acceso a un servicio HTTP en una máquina remota.           |
| **Reverse Tunneling**             | Permite acceder a servicios del cliente desde el servidor.                                           | Acceso a SSH del cliente desde el servidor.                |
| **Conexión a través de Proxy**    | Conexión a través de proxies HTTP y SOCKS5.                                                          | Conexión a través de un proxy SOCKS5 en un entorno restringido. |
| **Combinación con Metasploit**    | Uso de Chisel para abrir túneles y red
