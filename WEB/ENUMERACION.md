
# Enumeración de usuarios y contraseñas
-Prestar atención a la pagina de registro y los mensajes que da ej(este usuario ya existe, la contraseña debe ser superior a 8 caracteres y contener una letra mayúscula)
-Prestar atención a la sección "olvide mi contraseña"
-Prestar atención a mensajes de error al intentar logarnos
-Brechas de seguridad pasadas

# Verbose Errors
-a veces las paginas web nos devuelven mensajes de error que nos pueden aportar informacion sobre la base de datos, el servidor que usan,rutas internas, usuarios existentes etc
## ¿como causar estos errores?

-formulario de login
-SQL injection (añadiendo ' o " en un campo)
-Path Traversal (../../../)


# Enumeración usando wayback machine
-La herramienta se encuentra en el siguiente repo https://github.com/tomnomnom/waybackurls

se ejecutan los siguientes comandos:

```
sudo apt install golang-go -y
go build
./waybackurls {objetivo}
```


