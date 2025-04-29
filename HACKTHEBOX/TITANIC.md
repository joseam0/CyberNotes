
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

viendo la documentación de gitea vemos que la base de datos se aloja en local en esta ruta
![[Pasted image 20250429011252.png]]

hacemos la peticion a traves del lfi y obtenemos el .db![[Pasted image 20250429011941.png]]

despues buscamos en las tablas con sqlite browser y extraemos la siguiente info de la tabla user con la siguiente query

```sql
SELECT name,passwd,passwd_hash_algo,salt,rands from user;
```

| name              | passwd                                                                                                   | passwd_hash_algo    | salt                                 | rands                                |     |     |
| ----------------- | -------------------------------------------------------------------------------------------------------- | ------------------- | ------------------------------------ | ------------------------------------ | --- | --- |
| administrator<br> | cba20ccf927d3ad0567b68161732d3fbca098ce886bbc923b4062a3960d459c08d2dfc063b2406ac9207c980c47c5d017136<br> | pbkdf2$50000$50<br> | 2d149e5fbd1b20cf31db3e3c6a28fc9b<br> | 70a5bd0c1a5d23caa49030172cdcabdc<br> |     |     |
| administrator<br> | e531d398946137baea70ed6a680a54385ecff131309c0bd8f225f284406b7cbc8efc5dbef30bf1682619263444ea594cfb56<br> | pbkdf2$50000$50<br> | 8bf3e3452b78544f8bee9400d6936d34<br> | 0ce6f07fc9b557bc070fa7bef76a0d15<br> |     |     |



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




