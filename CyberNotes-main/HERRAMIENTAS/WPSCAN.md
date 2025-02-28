


| Opción                   | Descripción                                                              | Ejemplo de Uso                                                  |
|--------------------------|--------------------------------------------------------------------------|-----------------------------------------------------------------|
| `--url`                  | Especifica la URL del objetivo (incluyendo el prefijo "http://").        | `wpscan --url http://{url}`                                     |
| `--enumerate`            | Enumera información específica (plugins, temas, usuarios, etc.).          | `wpscan --url http://{url} --enumerate plugins,themes`           |
| `--detection-mode`       | Modo de detección (agresivo o seguro).                                   | `wpscan --url http://{url} --detection-mode aggressive`         |
| `--disable-tls-checks`   | Deshabilita las comprobaciones de TLS/SSL.                               | `wpscan --url http://{url} --disable-tls-checks`                 |
| `--proxy`                | Especifica un proxy para las solicitudes.                                | `wpscan --url http://{url} --proxy http://proxy.example.com:8080` |
| `--user-agent`           | Especifica el User-Agent para las solicitudes.                          | `wpscan --url http://{url} --user-agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3"` |
| `--random-agent`         | Utiliza un User-Agent aleatorio para cada solicitud.                    | `wpscan --url http://{url} --random-agent`                      |
| `--follow-redirection`   | Sigue las redirecciones.                                                | `wpscan --url http://{url} --follow-redirection`                |
| `--ignore-main-redirect` | Ignora la redirección principal de la URL.                              | `wpscan --url http://{url} --ignore-main-redirect`              |
| `--disable-accept-header` | Deshabilita la comprobación del encabezado Accept-Language.             | `wpscan --url http://{url} --disable-accept-header`             |
| `--wp-content-dir`       | Especifica el nombre del directorio wp-content.                         | `wpscan --url http://{url} --wp-content-dir custom-content`    |
| `--wp-plugins-dir`       | Especifica el nombre del directorio de plugins.                         | `wpscan --url http://{url} --wp-plugins-dir custom-plugins`    |
| `--wp-themes-dir`        | Especifica el nombre del directorio de temas.                           | `wpscan --url http://{url} --wp-themes-dir custom-themes`      |
| `--force`                | Fuerza la exploración, incluso si el objetivo no se identifica como WordPress. | `wpscan --url http://{url} --force`                            |
| `--log`                  | Guarda la salida en un archivo de registro.                             | `wpscan --url http://{url} --log wpscan.log`                   |
| `--disable-referer`      | Deshabilita el encabezado Referer en las solicitudes.                  | `wpscan --url http://{url} --disable-referer`                  |
| `--disable-prediction`   | Deshabilita la predicción de URL basada en la redirección.             | `wpscan --url http://{url} --disable-prediction`               |
| `--dbs`                  | Enumera las bases de datos de vulnerabilidades conocidas.               | `wpscan --url http://{url} --dbs`                              |
| `--update`               | Actualiza WPScan a la última versión desde el repositorio oficial.     | `wpscan --update`                                              |
| `--version`              | Muestra la versión actual de WPScan.                                    | `wpscan --version`                                             |
