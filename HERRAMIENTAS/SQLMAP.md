| Opción            | Descripción                                                                                 | Ejemplo                                                         |
| ----------------- | ------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| `-u, --url`       | Especifica la URL del objetivo.                                                             | `sqlmap -u "http://{HOST}/page?id=1"`                           |
| `--data`          | Especifica datos POST para las solicitudes.                                                 | `sqlmap --data "user=admin&password=pass"`                      |
| `--cookie`        | Especifica cookies para la solicitud.                                                       | `sqlmap --cookie "sessionid=123456"`                            |
| `--level`         | Nivel de riesgo de la prueba (1-5).                                                         | `sqlmap --url "http://{HOST}" --level 3`                        |
| `--risk`          | Nivel de riesgo de la prueba (0-3).                                                         | `sqlmap --url "http://{HOST}" --risk 2`                         |
| `-p`              | Especifica parámetros de URL a analizar.                                                    | `sqlmap -u "http://{HOST}/page?id=1" -p id`                     |
| `--dbms`          | Especifica el sistema de gestión de bases de datos (DBMS) objetivo.                         | `sqlmap --url "http://{HOST}" --dbms mysql`                     |
| `--tables`        | Enumera las tablas de la base de datos.                                                     | `sqlmap --url "http://{HOST}" --tables`                         |
| `--columns`       | Enumera las columnas de una tabla.                                                          | `sqlmap --url "http://{HOST}" -D dbname -T tablename --columns` |
| `--dump`          | Extrae datos de la base de datos.                                                           | `sqlmap --url "http://{HOST}" --dump`                           |
| `--threads`       | Número de hilos concurrentes.                                                               | `sqlmap --url "http://{HOST}" --threads 5`                      |
| `--tor`           | Utiliza la red Tor para realizar solicitudes anónimas.                                      | `sqlmap --url "http://{HOST}" --tor`                            |
| `--proxy`         | Especifica un proxy para enrutar las solicitudes a través de él.                            | `sqlmap --url "http://{HOST}" --proxy http://proxy.{HOST}:8080` |
| `--time-sec`      | Tiempo de espera para las solicitudes de la base de datos (en segundos).                    | `sqlmap --url "http://{HOST}" --time-sec 5`                     |
| `--batch`         | Modo de lote (sin preguntas).                                                               | `sqlmap --url "http://{HOST}" --batch`                          |
| `--flush-session` | Elimina todas las variables de sesión entre ejecuciones.                                    | `sqlmap --url "http://{HOST}" --flush-session`                  |
| `--wizard`        | Inicia el asistente interactivo.                                                            | `sqlmap --wizard`                                               |
| `--update`        | Actualiza SQLMap a la última versión desde el repositorio oficial.                          | `sqlmap --update`                                               |
| `--forms`         | Enumera y prueba formularios web en la URL dada.                                            | `sqlmap --url "http://{HOST}" --forms`                          |
| `--os-shell`      | Inicia una shell interactiva del sistema operativo.                                         | `sqlmap --url "http://{HOST}" --os-shell`                       |
| `--sql-shell`     | Inicia una shell interactiva de SQLMap.                                                     | `sqlmap --url "http://{HOST}" --sql-shell`                      |
| `--tamper`        | Utiliza scripts de codificación/decodificación personalizados.                              | `sqlmap --url "http://{HOST}" --tamper=mycustomscript`          |
| `--csrf-token`    | Especifica un token CSRF para las solicitudes POST.                                         | `sqlmap --url "http://{HOST}" --csrf-token=token123`            |
| `--eval`          | Evalúa una expresión SQL después de la inyección.                                           | `sqlmap --url "http://{HOST}" --eval "print 'Hello, SQLMap!'"`  |
| `--exclude`       | Excluye ciertos tipos de pruebas de seguridad (por ejemplo, no probar inyección de cadena). | `sqlmap --url "http://{HOST}" --exclude="OS,TIME"`              |
| `--batch`         | Modo de lote (sin preguntas).                                                               | `sqlmap --url "http://{HOST}" --batch`                          |
| `--random-agent`  | Utiliza un agente de                                                                        |                                                                 |
