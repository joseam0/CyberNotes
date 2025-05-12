
<font color="#00b0f0">IP</font>: 10.10.11.58
<font color="#00b0f0">HOSTNAME</font>:
<font color="#00b0f0">S.O</font>:LINUX
<font color="#00b0f0">DIFICULTAD</font>:

WRITEUP

tras la enumeracion con nmap vemos varias cosas interesantes de la aplicacion web, por un lado que contiene un .git, por otro lado, en el robots.txt se descubren muchas rutas


```bash
User-agent: *
Crawl-delay: 10
# Directories
Disallow: /core/
Disallow: /profiles/
# Files
Disallow: /README.md
Disallow: /web.config
# Paths (clean URLs)
Disallow: /admin
Disallow: /comment/reply
Disallow: /filter/tips
Disallow: /node/add
Disallow: /search
Disallow: /user/register
Disallow: /user/password
Disallow: /user/login
Disallow: /user/logout
# Paths (no clean URLs)
Disallow: /?q=admin
Disallow: /?q=comment/reply
Disallow: /?q=filter/tips
Disallow: /?q=node/add
Disallow: /?q=search
Disallow: /?q=user/password
Disallow: /?q=user/register
Disallow: /?q=user/login
Disallow: /?q=user/logout

```


clonamos el repo con git dumper



![[Pasted image 20250507193930.png]]

## <font color="#00b0f0">USERS</font>

| USER              | PASSWORD            | PERMISOS          |
| ----------------- | ------------------- | ----------------- |
| dogBackDropSystem |                     | usuario de la web |
| tiffany@dog.htb   | BackDropJ2024DS2024 |                   |


## <font color="#00b0f0">ENUMERACION</font>

Comenzamos enumerando con nmap usando el siguiente comando

```
sudo nmap 10.10.11.58 -Pn -T4 -sSCV -p- --open -oA nmap_dog -vv
```


| Puerto | Servicio | Version |
| ------ | -------- | ------- |
|        |          |         |
|        |          |         |
|        |          |         |


Usamos gobuster para enumerar directorios por fuzzing con el siguente comando 

```
gobuster dir -u http://dog.htb -r  -e  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 30 
```



## <font color="#00b0f0">SERVICIOS INTERESANTES</font>




