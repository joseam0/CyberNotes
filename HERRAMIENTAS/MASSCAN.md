
| Opción          | Descripción                                          | Ejemplo de Uso                                        |
|------------------|------------------------------------------------------|--------------------------------------------------------|
| -p (Ports)       | Especifica los puertos a escanear.                   | `masscan -p1-65535 192.168.1.1`                        |
| -oL (Output)     | Guarda los resultados en formato lista.              | `masscan -p80,443 192.168.1.0/24 -oL scan_results.txt` |
| -oJ (JSON Output) | Guarda los resultados en formato JSON.               | `masscan -p1-65535 192.168.1.1 -oJ scan_results.json`  |
| -iL (Input List) | Lee las direcciones IP y rangos desde un archivo.    | `masscan -p80 -iL target_list.txt`                     |
| -exclude         | Excluye direcciones IP específicas del escaneo.     | `masscan -p1-65535 192.168.1.1 --exclude 192.168.1.2`  |
| -rate (Packets per second) | Establece la velocidad de escaneo.            | `masscan -p80 192.168.1.1 --rate 1000`                |
| --banners        | Captura banners de servicios en puertos abiertos.    | `masscan -p21,22,23 192.168.1.1 --banners`            |
| --source-ip      | Especifica la dirección IP de origen para escanear.  | `masscan -p80 192.168.1.1 --source-ip 192.168.1.2`    |
| --router-mac     | Spoofea la dirección MAC del router de destino.      | `masscan -p80 192.168.1.1 --router-mac 00:11:22:33:44:55` |
| --ping           | Realiza ping antes de escanear para verificar host. | `masscan -p80 192.168.1.1 --ping`                     |
| --open-only      | Muestra solo los puertos abiertos en la salida.      | `masscan -p1-65535 192.168.1.1 --open-only`            |
| --wait           | Espera el tiempo especificado antes de escanear.    | `masscan -p80 192.168.1.1 --wait 10`                  |
| --randomize-hosts| Escanea direcciones IP en orden aleatorio.           | `masscan -p80 192.168.1.0/24 --randomize-hosts`       |
| --echo           | Muestra en la salida estándar la línea de comando generada. | `masscan -p80 192.168.1.1 --echo`               |
