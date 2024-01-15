


| Flag             | Explicación                                                                                      | Ejemplo                                                                                    |
|------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| `-p`             | Especifica los puertos a escanear.                                                               | `nmap -p 80,443 {HOST}`                                                               |
| `-sS`            | Realiza un escaneo de tipo SYN (half-open).                                                    | `nmap -sS {HOST}`                                                                    |
| `-A`             | Realiza un escaneo agresivo que incluye detección de sistemas operativos, servicios y versiones. | `nmap -A {HOST}`                                                                     |
| `-sV`            | Detección de versiones de servicios.                                                           | `nmap -sV {HOST}`                                                                    |
| `-O`             | Detección de sistema operativo.                                                                | `nmap -O {HOST}`                                                                     |
| `-T`             | Ajusta el nivel de agresividad del escaneo (0-5).                                              | `nmap -T4 {HOST}`                                                                   |
| `--script`       | Ejecuta scripts NSE (Nmap Scripting Engine).                                                   | `nmap --script vuln {HOST}`                                                          |
| `-n`             | Desactiva la resolución de DNS.                                                                | `nmap -n {HOST}`                                                                     |
| `--open`         | Muestra solo los puertos abiertos.                                                             | `nmap --open {HOST}`                                                                |
| `--reason`       | Muestra la razón por la cual un puerto está en un estado particular.                           | `nmap --reason {HOST}`                                                              |
| `-v`             | Aumenta el nivel de verbosidad (puede usarse varias veces).                                   | `nmap -v {HOST}`                                                                    |
| `-sU`            | Realiza un escaneo de tipo UDP.                                                                | `nmap -sU {HOST}`                                                                    |
| `-sT`            | Realiza un escaneo de tipo TCP completo.                                                       | `nmap -sT {HOST}`                                                                    |
| `-h`             | Muestra la ayuda y la información sobre los parámetros.                                        | `nmap -h`                                                                               |
| `-Pn`            | Ignora la verificación de host en línea y escanea directamente.                                 | `nmap -Pn {HOST}`                                                                    |
| `--max-retries`  | Especifica el número máximo de reintentos después de un fallo de host o escaneo de puerto.       | `nmap --max-retries 3 {HOST}`                                                       |
| `--min-rate`     | Establece la velocidad mínima de envío de paquetes.                                             | `nmap --min-rate 1000 {HOST}`                                                       |
| `--max-rate`     | Establece la velocidad máxima de envío de paquetes.                                             | `nmap --max-rate 5000 {HOST}`                                                       |
| `--traceroute`   | Muestra la ruta que los paquetes toman para llegar al host.                                    | `nmap --traceroute {HOST}`                                                          |
| `--ttl`          | Modifica el valor de TTL (Time To Live) en los paquetes enviados.                               | `nmap --ttl 64 {HOST}`                                                              |
| `--defeat-rst-ratelimit` | Intenta evadir la limitación de velocidad de las respuestas RST.                              | `nmap --defeat-rst-ratelimit {HOST}`                                               |
| `--iflist`       | Muestra una lista de interfaces y sus direcciones IP asociadas.                                | `nmap --iflist`                                                                         |
| `--packet-trace` | Muestra una traza detallada de los paquetes enviados y recibidos.                               | `nmap --packet-trace {HOST}`                                                        |
| `--spoof-mac`    | Especifica una dirección MAC falsa para el escaneo.                                            | `nmap --spoof-mac 00:11:22:33:44:55 {HOST}`                                         |
| `--script-updatedb` | Actualiza la base de datos de scripts NSE.                                                  | `nmap --script-updatedb`                                                                |
| `--scan-delay`   | Añade un retraso entre los escaneos para evitar la detección.                                    | `nmap --scan-delay 2s {HOST}`                                                       |
| `--source-port`  | Especifica el puerto de origen para los paquetes enviados.                                      | `nmap --source-port 12345 {HOST}`                                                   |
| `--data-length`  | Envía paquetes con datos adicionales específicos de longitud.                                   | `nmap --data-length 100 {HOST}`                                                     |
| `--badsum`       | Genera sumas de comprobación de IP incorrectas.                                                | `nmap --badsum {HOST}`                                                              |
| `-6`             | Realiza un escaneo solo sobre IPv6.                                                            | `nmap -6 {HOST}`                                                                    |
| `--script-args`  | Permite pasar argumentos a los scripts NSE.                                                    | `nmap --script-args vuln.param=value {HOST}`                                         |
| `--script-macros` | Define macros para los scripts NSE.                                                           | `nmap --script-macros nmapcustom=/path/to/custom.nse`                                    |
| `--script-help`  | Muestra información de ayuda sobre scripts específicos.                                        | `nmap --script-help scriptname`                                                          |
| `-oN`            | Guarda la salida en formato normal.                                                            | `nmap -oN output.txt {HOST}`                                                        |
| `-oX`            | Guarda la salida en formato XML.                                                               | `nmap -oX output.xml {HOST}`                                                        |


###### SCRIPTS

| Script                            | Descripción                                                                                     | Ejemplo de Uso                                               |
|-----------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `vuln`                            | Realiza pruebas de vulnerabilidad contra el objetivo.                                          | `nmap --script vuln {HOST}`                             |
| `http-enum`                       | Enumera información sobre servidores web, como directorios y archivos.                         | `nmap --script http-enum {HOST}`                        |
| `ssl-enum-ciphers`                | Enumera los cifrados SSL/TLS admitidos por un servicio.                                         | `nmap --script ssl-enum-ciphers {HOST}`                 |
| `dns-zone-transfer`               | Intenta realizar una transferencia de zona DNS desde un servidor DNS.                           | `nmap --script dns-zone-transfer {HOST}`               |
| `ftp-anon`                        | Intenta realizar conexión FTP anónima.                                                         | `nmap --script ftp-anon {HOST}`                        |
| `smb-vuln-ms17-010`               | Detecta si un sistema es vulnerable a EternalBlue (MS17-010).                                  | `nmap --script smb-vuln-ms17-010 {HOST}`               |
| `smtp-enum-users`                 | Enumera usuarios válidos en un servidor SMTP.                                                  | `nmap --script smtp-enum-users {HOST}`                 |
| `mysql-info`                      | Obtiene información sobre un servidor MySQL.                                                   | `nmap --script mysql-info {HOST}`                      |
| `snmp-info`                       | Obtiene información sobre un servicio SNMP.                                                    | `nmap --script snmp-info {HOST}`                       |
| `http-vuln-cve2017-5638`          | Detecta la vulnerabilidad de Apache Struts (CVE-2017-5638).                                     | `nmap --script http-vuln-cve2017-5638 {HOST}`          |
| `http-robots.txt`                 | Muestra el contenido del archivo `robots.txt` de un servidor web.                               | `nmap --script http-robots.txt {HOST}`                 |
| `ssh-brute`                       | Realiza un ataque de fuerza bruta contra el servicio SSH.                                      | `nmap --script ssh-brute {HOST}`                       |
| `ldap-search`                     | Realiza búsquedas LDAP para obtener información sobre directorios y objetos.                   | `nmap --script ldap-search {HOST}`                     |
| `http-vuln-cve2014-3704`          | Detecta la vulnerabilidad de Drupalgeddon (CVE-2014-3704) en sitios web Drupal.                  | `nmap --script http-vuln-cve2014-3704 {HOST}`          |
| `ssl-heartbleed`                  | Detecta la vulnerabilidad Heartbleed en servidores que utilizan OpenSSL.                         | `nmap --script ssl-heartbleed {HOST}`                  |
