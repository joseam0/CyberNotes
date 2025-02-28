| Opción        | Descripción                                       | Ejemplo de Uso                                                      |
| ------------- | ------------------------------------------------- | ------------------------------------------------------------------- |
| `-m`          | Especifica el tipo de hash que se está atacando.  | `hashcat -m 1000 hashes.txt wordlist.txt`                           |
| `-a`          | Selecciona el modo de ataque.                     | `hashcat -a 0 hashes.txt wordlist.txt`                              |
| `-w`          | Muestra resultados en formato "machine-readable". | `hashcat -m 1000 -a 0 -w 3 hashes.txt wordlist.txt`                 |
| `--force`     | Ignora advertencias y forza la ejecución.         | `hashcat -m 1000 -a 0 --force hashes.txt wordlist.txt`              |
| `--session`   | Guarda/restaura el estado de la sesión.           | `hashcat -m 1000 -a 0 --session=session1 hashes.txt wordlist.txt`   |
| `--increment` | Activa el modo de ataque incremental.             | `hashcat -m 1000 -a 3 hashes.txt ?a?a?a?a?a?a?a?a?a?a`              |
| `--status`    | Muestra el estado actual del ataque.              | `hashcat -m 1000 -a 0 --status`                                     |
| `--gpu-temps` | Muestra la temperatura de las GPUs.               | `hashcat -m 1000 -a 0 --gpu-temps`                                  |
| `--benchmark` | Realiza una prueba de rendimiento.                | `hashcat -m 1000 -a 0 --benchmark`                                  |
| `--restore`   | Restaura una sesión previamente guardada.         | `hashcat -m 1000 -a 0 --restore`                                    |
| `--outfile`   | Especifica el archivo de salida para contraseñas. | `hashcat -m 1000 -a 0 hashes.txt wordlist.txt --outfile=output.txt` |


# Distintos tipos de hashes junto con sus  codigos


| Código de Modo | Tipo de Hash                       |
|-----------------|------------------------------------|
| 0               | MD5 (Message Digest Algorithm 5)   |
| 1000            | NTLM (Microsoft NT LAN Manager)    |
| 500             | SHA-1 (Secure Hash Algorithm 1)    |
| 100             | SHA-256 (Secure Hash Algorithm 256-bit) |
| 1800            | SHA-512 (Secure Hash Algorithm 512-bit) |
| 10              | MD5 con Sal y Concatenación        |
| 20              | MD5 con Sal y Concatenación        |
| 30              | MD5 con Sal y UTF-16 LE            |
| 40              | MD5 con Sal y UTF-16 LE            |
| 110             | SHA-1 con Sal y Concatenación      |
| 120             | SHA-1 con Sal y Concatenación      |
| 130             | SHA-1 con Sal y UTF-16 LE           |
| 140             | SHA-1 con Sal y UTF-16 LE           |
| 300             | SHA-384 con Sal y Concatenación    |
| 1400            | SHA-512 Crypt (SHA-512 (Unix))     |

