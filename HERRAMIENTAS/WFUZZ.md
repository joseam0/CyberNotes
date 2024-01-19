



| Opción                | Descripción                                       | Ejemplo de Uso                                                |
|-----------------------|---------------------------------------------------|---------------------------------------------------------------|
| `-c`                  | Especifica el código de estado a considerar válido.| `wfuzz -c --hc=404,403 -w /path/to/wordlist.txt http://example.com/FUZZ` |
| `-w`                  | Especifica la ruta al archivo de palabras clave.  | `wfuzz -w /path/to/wordlist.txt http://example.com/FUZZ`      |
| `-z`                  | Especifica la fuente de datos para sustituir FUZZ.| `wfuzz -w /path/to/wordlist.txt -z file,/path/to/other_wordlist.txt http://example.com/FUZZ` |
| `-d`                  | Envia datos POST con el formato "nombre=valor".   | `wfuzz -w /path/to/wordlist.txt -d "user=admin&pass=FUZZ" http://example.com/login` |
| `-H`                  | Especifica encabezados HTTP adicionales.           | `wfuzz -w /path/to/wordlist.txt -H "Authorization: Bearer FUZZ" http://example.com/api/resource` |
| `--hh`                | Filtra respuestas basadas en encabezados HTTP.     | `wfuzz -w /path/to/wordlist.txt --hh=Location http://example.com/FUZZ` |
| `--hc`                | Filtra respuestas basadas en códigos de estado.    | `wfuzz -w /path/to/wordlist.txt --hc=200,204 http://example.com/FUZZ` |
| `--hl`                | Filtra respuestas basadas en longitud de contenido.| `wfuzz -w /path/to/wordlist.txt --hl=100 http://example.com/FUZZ` |
| `--hs`                | Filtra respuestas basadas en una cadena en el contenido. | `wfuzz -w /path/to/wordlist.txt --hs="Error" http://example.com/FUZZ` |
| `--hw`                | Filtra respuestas basadas en el ancho del contenido.| `wfuzz -w /path/to/wordlist.txt --hw=50 http://example.com/FUZZ` |
| `--sc`                | Imprime los primeros N caracteres de cada respuesta.| `wfuzz -w /path/to/wordlist.txt --sc=100 http://example.com/FUZZ` |
| `--hc`                | Filtra respuestas basadas en códigos de estado.    | `wfuzz -w /path/to/wordlist.txt --hc=200,204 http://example.com/FUZZ` |
| `--hl`                | Filtra respuestas basadas en longitud de contenido.| `wfuzz -w /path/to/wordlist.txt --hl=100 http://example.com/FUZZ` |
| `--hs`                | Filtra respuestas basadas en una cadena en el contenido. | `wfuzz -w /path/to/wordlist.txt --hs="Error" http://example.com/FUZZ` |
| `--hw`                | Filtra respuestas basadas en el ancho del contenido.| `wfuzz -w /path/to/wordlist.txt --hw=50 http://example.com/FUZZ` |
| `--sc`                | Imprime los primeros N caracteres de cada respuesta.| `wfuzz -w /path/to/wordlist.txt --sc=100 http://example.com/FUZZ` |
