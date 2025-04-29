Hydra es capaz de atacar varios servicios y protocolos, incluyendo SSH, FTP, HTTP, HTTPS, Telnet, MySQL, VNC, entre otros.


| Opción                  | Descripción                                                       | Ejemplo de Uso                                                                                 |
| ----------------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `-l, --login`           | Especifica el nombre de usuario o archivo de nombres de usuario.  | `hydra -l username -P /path/to/passwords.txt ssh://{HOST}`                                     |
| `-L, --user`            | Especifica un nombre de usuario o archivo de nombres de usuario.  | `hydra -L /path/to/usernames.txt -p password ftp://{HOST}`                                     |
| `-p, --password`        | Especifica la contraseña o archivo de contraseñas.                | `hydra -l username -p password123 ftp://{HOST}`                                                |
| `-P, --pass-list`       | Especifica un archivo de contraseñas.                             | `hydra -l username -P /path/to/passwords.txt ssh://{HOST}`                                     |
| `-e, --empty-passwords` | Intenta utilizar una cadena de contraseña vacía.                  | `hydra -l username -P /path/to/passwords.txt -e ns ssh://{HOST}`                               |
| `-s, --service`         | Especifica el servicio a atacar (e.g., ftp, ssh, http-get).       | `hydra -l username -P /path/to/passwords.txt -s 22 ssh://{HOST}`                               |
| `-t, --threads`         | Número de hilos concurrentes.                                     | `hydra -l username -P /path/to/passwords.txt -t 8 ssh://{HOST}`                                |
| `-vV, --version`        | Muestra la versión de Hydra.                                      | `hydra --version`                                                                              |
| `-h, --help`            | Muestra información de ayuda.                                     | `hydra --help`                                                                                 |
| `-o, --output`          | Guarda la salida en un archivo.                                   | `hydra -l username -P /path/to/passwords.txt -o result.txt ftp://{HOST}`                       |
| `--f, --force`          | Suprime los mensajes de advertencia.                              | `hydra -l username -P /path/to/passwords.txt --f ssh://{HOST}`                                 |
| `--S, --start-at`       | Comienza en un índice específico en el archivo de contraseñas.    | `hydra -l username -P /path/to/passwords.txt --S 10 ssh://{HOST}`                              |
| `--F, --finish-at`      | Termina en un índice específico en el archivo de contraseñas.     | `hydra -l username -P /path/to/passwords.txt --F 50 ssh://{HOST}`                              |
| `--oL, --out-file`      | Guarda resultados en un archivo en formato compatible con Medusa. | `hydra -L /path/to/usernames.txt -P /path/to/passwords.txt -oL result.txt ftp://{HOST}`        |
| `--crack-status`        | Muestra el estado del proceso de crack después de la prueba.      | `hydra -l username -P /path/to/passwords.txt --crack-status ssh://{HOST}`                      |
| `--resume`              | Reanuda una sesión de craqueo anterior.                           | `hydra -l username -P /path/to/passwords.txt --resume ssh://{HOST}`                            |
| `--try-first-pass`      | Prueba una contraseña específica primero.                         | `hydra -l username --try-first-pass admin ftp://{HOST}`                                        |
| `--timeout`             | Tiempo de espera para la respuesta del servidor.                  | `hydra -l username -P /path/to/passwords.txt --timeout 5 ssh://{HOST}`                         |
| `--retries`             | Número de reintentos antes de considerar un servicio inaccesible. | `hydra -l username -P /path/to/passwords.txt --retries 3 ssh://{HOST}`                         |
| `--failures`            | Número máximo de fallos antes de dar por terminada la prueba.     | `hydra -l username -P /path/to/passwords.txt --failures 10 ssh://{HOST}`                       |
| `--encoding`            | Codificación de caracteres para las pruebas de contraseña.        | `hydra -l username -P /path/to/passwords.txt --encoding utf-8 ssh://{HOST}`                    |
| `--stdin`               | Lee contraseñas desde la entrada estándar.                        | `hydra -l username -P - ssh://{HOST} < /path/to/passwords.txt`                                 |
| `--reconnect`           | Reconecta después de cada intento de autenticación fallido.       | `hydra -l username -P /path/to/passwords.txt --reconnect ssh://{HOST}`                         |
| `--e, --eF`             | Ejecuta comandos después de un intento exitoso.                   | `hydra -l username -P /path/to/passwords.txt --e "echo 'Password found: ^PASS^'" ssh://{HOST}` |
| `--t, --tF`             | Ejecuta comandos después de un intento fallido.                   | `hydra -l username -P /path/to/passwords.txt --t "echo 'Failed                                 |
