


Herramienta que sirve para hacer FUZZING  a una web, para el descubrimiento de directorios o subdominios.

Similar a herramientas como 


| Flag | Explicación | Ejemplo |
| ---- | ---- | ---- |
| `-w, --wordlist` | Permite especificar la ubicación del diccionario. | `gobuster dir -u http://a.com -w /a/dict.txt` |
| `-t, --threads` | Controla el número de hilos utilizados para la búsqueda. | `gobuster dir -u http://a.com -w /a/dict.txt -t 20` |
| `-e, --expanded` | Muestra códigos de estado extendidos y tamaños de respuesta. | `gobuster dir -u http://a.com -w /a/dict.txt -e` |
| `-x, --extensions` | Especifica extensiones de archivo para buscar. | `gobuster dir -u http://a.com -w /a/dict.txt -x php,html,txt` |
| `-s, --status` | Filtra por códigos de estado HTTP. | `gobuster dir -u http://a.com -w /a/dict.txt -s 200,204,301` |
| `-a, --useragent` | Especifica el User-Agent en las solicitudes HTTP. | `gobuster dir -u http://a.com -w /a/dict.txt -a "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"` |
| `-c, --cookies` | Permite establecer cookies en las solicitudes. | `gobuster dir -u http://a.com -w /a/dict.txt -c "sessionid=123456;user=admin"` |
| `-k, --insecure` | Ignora los errores de certificado SSL. | `gobuster dir -u https://a.com -w /a/dict.txt -k` |
| `-l, --wildcard` | Muestra resultados incluso si hay respuestas wildcard. | `gobuster dir -u http://a.com -w /a/dict.txt -l` |
| `-f, --addslash` | Agrega una barra inclinada ("/") al final de cada directorio. | `gobuster dir -u http://a.com -w /a/dict.txt -f` |
| `-d, --dns` | Realiza una búsqueda de subdominios mediante resolución DNS. | `gobuster dns -d a.com -w /path/to/subdomain_wordlist.txt` |
| `-r, --follow-redirect` | Sigue automáticamente redireccionamientos. | `gobuster dir -u http://a.com -w /a/dict.txt -r` |
| `--timeout` | Establece el tiempo de espera para cada solicitud en segundos. | `gobuster dir -u http://a.com -w /a/dict.txt --timeout 20` |
| `--noprogress` | Desactiva la barra de progreso para una salida más limpia. | `gobuster dir -u http://a.com -w /a/dict.txt --noprogress` |
| `--proxy` | Especifica un proxy para enrutar las solicitudes a través de él. | `gobuster dir -u http://a.com -w /a/dict.txt --proxy http://proxy.a.com:8080` |
| `--quiet` | Muestra solo los resultados encontrados, sin información adicional. | `gobuster dir -u http://a.com -w /a/dict.txt --quiet` |
| `--userpass` | Especifica un nombre de usuario y contraseña para la autenticación básica. | `gobuster dir -u http://a.com -w /a/dict.txt --userpass admin:password` |
| `--headers` | Agrega encabezados personalizados a las solicitudes HTTP. | `gobuster dir -u http://a.com -w /a/dict.txt --headers "Authorization: Bearer <token>"` |
| `--exclude` | Excluye ciertos códigos de estado HTTP de los resultados. | `gobuster dir -u http://a.com -w /a/dict.txt --exclude 403,404` |



