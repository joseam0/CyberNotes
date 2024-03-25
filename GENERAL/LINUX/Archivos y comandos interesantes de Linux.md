

como compartirse un servidor http para intercambiar archivos usando python
desde la carpeta donde tenemos los archivos lanzamos en una terminal
`python3 -m http.server 8080`

Una vez lo tengamos a la escucha podremos descargarnos el contenido
### usando curl
`curl -O http://direccion_ip_del_servidor:puerto/ruta/al/archivo`
### o usando wget
`wget http://direccion_ip_del_servidor:puerto/ruta/al/archivo`

