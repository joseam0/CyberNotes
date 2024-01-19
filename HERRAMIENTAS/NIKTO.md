| Opción              | Descripción                                                | Ejemplo de Uso                                   |
|----------------------|------------------------------------------------------------|--------------------------------------------------|
| -h (Host)            | Especifica el host o la dirección IP del servidor web.     | `nikto -h example.com`                           |
| -p (Port)            | Especifica el puerto del servidor web.                     | `nikto -h example.com -p 8080`                   |
| -ssl                 | Habilita el escaneo sobre SSL/TLS.                          | `nikto -h example.com -ssl`                      |
| -id (Authentication) | Proporciona credenciales para la autenticación.           | `nikto -h example.com -id admin:password`        |
| -evasion (Evasion Techniques) | Utiliza técnicas de evasión para evitar la detección. | `nikto -h example.com -evasion 2`               |
| -mutate (Mutation Techniques) | Aplica técnicas de mutación en las solicitudes.        | `nikto -h example.com -mutate 1,2,3`            |
| -o (Output)          | Especifica el archivo de salida para el informe.           | `nikto -h example.com -o nikto_report.txt`       |
| -config (Config File) | Carga opciones desde un archivo de configuración.         | `nikto -h example.com -config nikto.conf`        |
| -list-plugins        | Muestra la lista de plugins disponibles.                  | `nikto -list-plugins`                           |
| -update              | Actualiza la base de datos de Nikto.                       | `nikto -update`                                |
| -Plugins             | Ejecuta escaneos específicos basados en categorías de plugins. | `nikto -h example.com -Plugins test+`         |
| -Tuning              | Ajusta el perfil de escaneo (paranoid, stealth, etc.).     | `nikto -h example.com -Tuning 2`                |
| -timeout            | Establece el tiempo de espera para solicitudes en segundos. | `nikto -h example.com -timeout 10`             |
| -maxtime            | Establece el tiempo máximo de ejecución en minutos.        | `nikto -h example.com -maxtime 30`             |
| -pause              | Establece el tiempo de pausa entre solicitudes en segundos. | `nikto -h example.com -pause 2`               |
| -useragent          | Especifica un User-Agent personalizado para las solicitudes. | `nikto -h example.com -useragent "MiNavegador"` |
