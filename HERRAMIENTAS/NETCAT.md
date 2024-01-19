

| Opción             | Descripción                                         | Ejemplo de Uso                                      |
|--------------------|-----------------------------------------------------|------------------------------------------------------|
| -l                 | Modo escucha, para esperar conexiones.               | `nc -lvp 4444`                                      |
| -p (Port)          | Especifica el puerto para la conexión.              | `nc -lvp 4444`                                      |
| -n                 | No resuelve nombres de dominio en direcciones IP.   | `nc -n -lvp 4444`                                   |
| -v (Verbose)       | Modo verbose, muestra detalles sobre la conexión.   | `nc -lvp 4444 -v`                                   |
| -e (Program)       | Ejecuta un programa después de aceptar la conexión. | `nc -lvp 4444 -e /bin/bash`                         |
| -u (UDP)           | Utiliza el protocolo UDP en lugar de TCP.           | `nc -u -lvp 4444`                                   |
| -w (Timeout)       | Establece un tiempo de espera para la conexión.     | `nc -lvp 4444 -w 30`                                |
| -k (Keep Open)      | Permite múltiples conexiones en modo escucha.        | `nc -lvp 4444 -k`                                   |
| -z (Zero I/O Mode)  | Escanea un puerto sin enviar datos.                 | `nc -zv 192.168.1.1 80`                             |
| -s (Source IP)     | Especifica la dirección IP de origen.               | `nc -s 192.168.1.2 -lvp 4444`                       |
| -c (Send/Receive)   | Permite enviar o recibir datos.                     | `nc -lvp 4444 < file_to_send`                       |
| -h (Help)           | Muestra información de ayuda.                       | `nc -h`                                             |
| -q (Quiet)          | Modo silencioso, suprime mensajes de salida.        | `nc -lvp 4444 -q 0`                                 |
| -d (Detach)         | Ejecuta Netcat en segundo plano (detached).         | `nc -lvp 4444 -e /bin/bash -d`                      |
| -z (Zero I/O Mode)  | Escanea puertos sin enviar datos (sin salida).      | `nc -zv 192.168.1.1 80`                             |
