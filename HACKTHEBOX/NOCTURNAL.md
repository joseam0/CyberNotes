
<font color="#00b0f0">IP</font>: 10.10.11.64
<font color="#00b0f0">HOSTNAME</font>:
<font color="#00b0f0">S.O</font>:LINUX
<font color="#00b0f0">DIFICULTAD</font>:

WRITEUP

tras registrarnos en la pagina y subir un archivo cualquiera vemos que para obtener esos archivos subidos se hace mediante una peticion GET con dos parametros. username y file


si iteramos el parametro username con el intruder de burp usando un diccionario con posibles nombres de usuario vemos lo siguiente
![[Pasted image 20250507231930.png]]
hay dos usuarios con los que la respuesta no tiene lenght 3268 si miramos los contenidos, los de peter no se pueden descargar ya que sera otro usuario de htb, pero amanda tiene un documento interesante llamado privacy.odt


## <font color="#00b0f0">USERS</font>

| USER | PASSWORD | PERMISOS |
| ---- | -------- | -------- |
|      |          |          |
|      |          |          |


## <font color="#00b0f0">ENUMERACION</font>

Comenzamos enumerando con nmap usando el siguiente comando

```
sudo nmap 10.10.11.64 -Pn -T4 -sSCV -p- --open -oA nmap_titanic -vv
```


| Puerto | Servicio | Version |
| ------ | -------- | ------- |
|        |          |         |
|        |          |         |


Usamos gobuster para enumerar directorios por fuzzing con el siguente comando 

```
gobuster dir -u http://nocturnal.htb -r  -e  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 30 
```

ningun resultado

## <font color="#00b0f0">SERVICIOS INTERESANTES</font>




