
<font color="#00b0f0">IP</font>: 10.10.11.55
<font color="#00b0f0">HOSTNAME</font>:
<font color="#00b0f0">S.O</font>:LINUX
<font color="#00b0f0">DIFICULTAD</font>:

WRITEUP


Después de hacer la enumeración añadimos titanic.htb al archivo /etc/hosts

vemos que hay una vulnerabilidad LFI ya que mediante path traversal podemos acceder al etc/passwd en el parametro ticket de /download

![[Pasted image 20250428223649.png]]

mandamos la peticion al intruder e iteramos sobre una lista de payloads
viendo su etc/hosts vemos que tiene un subdominio dev.titanic.htb
![[Pasted image 20250429003124.png]]
 

ahi tiene alojado un gitea, que es similar a un github

vemos que corre una base de datos y que tiene credenciales en plano en el repo
![[Pasted image 20250429011204.png]]
pero no tiene el puerto de mysql abierto

viendo la documentacion ed gitea vemos que la base de datos se alojha en local en esta ruta
![[Pasted image 20250429011252.png]]

hacemos la peticion a traves del lfi y obtenemos el .db![[Pasted image 20250429011941.png]]

despues buscamos en las tablas con sqlite browser

## <font color="#00b0f0">USERS</font>

| USER      | PASSWORD | PERMISOS |
| --------- | -------- | -------- |
| root      |          | root     |
| developer |          |          |


## <font color="#00b0f0">ENUMERACION</font>

Comenzamos enumerando con nmap usando el siguiente comando

```
sudo nmap 10.10.11.55 -Pn -T4 -sSCV -p- --open -oA nmap_titanic -vv
```


| Puerto | Servicio | Version |
| ------ | -------- | ------- |
| 22     | openssh  | 8.9     |
| 80     | Apache   | 2.4.52  |


Usamos gobuster para enumerar directorios por fuzzing con el siguente comando 

```
gobuster dir -u http://titanic.htb -r  -e  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 30 
```


con esto descubrimos varios directorios


/book

aqui se envia una solicitud POST con varios parametros y devuleve un numero de ticket (en las respuestas del server se ve que corre sobre un werkzeug que usa python)

/download

aqui en un parametro GET ves el ticket en formato json

## <font color="#00b0f0">SERVICIOS INTERESANTES</font>




