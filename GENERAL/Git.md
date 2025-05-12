


| **init** | *Inicializa un nuevo repositorio Git en el directorio actual* |  |
| ---- | ---- | ---- |
| **clone** | *Clona un repositorio Git existente, incluyendo todo su historial, a un nuevo directorio* |  |
| **status** | *Muestra el estado del repositorio, incluyendo cambios que no han sido añadidos o confirmados* |  |
| **add** | *Añade cambios en los archivos al área de preparación (staging area) para su próxima confirmación* |  |
| **commit** | *Guarda los cambios preparados en el historial del repositorio con un mensaje descriptivo* |  |
| **push** | *Envía los cambios confirmados de un repositorio local a un repositorio remoto* |  |
| **pull** | *Actualiza el repositorio local con los cambios del repositorio remoto, fusionándolos si es necesario* |  |
| **branch** | *Lista, crea, o elimina ramas en el repositorio* |  |
| **checkout / switch** | *Cambia de una rama a otra en el repositorio (checkout es usado en versiones anteriores de Git, switch en las más recientes)* |  |
| **merge** | *Combina los cambios de una rama en otra, generalmente la rama actual.* |  |
| **diff** | *Muestra las diferencias entre archivos, ramas, commits, etc* |  |
| **log** | *Muestra el historial de commits en el repositorio, incluyendo detalles como autor y fecha* |  |







AQUI abajo habra ejemplos (testing el plugin de obsidian) (otra prueba)






# init
Inicializa un nuevo repositorio Git en el directorio actual 

# clone
Clona un repositorio Git existente, incluyendo todo su historial, a un nuevo directorio

# status
Muestra el estado del repositorio, incluyendo cambios que no han sido añadidos o confirmados 

# add
Añade cambios en los archivos al área de preparación (staging area) para su próxima confirmación

# commit
Guarda los cambios preparados en el historial del repositorio con un mensaje descriptivo 

# push 
Envía los cambios confirmados de un repositorio local a un repositorio remoto 

# pull 
Actualiza el repositorio local con los cambios del repositorio remoto, fusionándolos si es necesario

# branch
Lista, crea, o elimina ramas en el repositorio 

# checkout / switch
Cambia de una rama a otra en el repositorio (checkout es usado en versiones anteriores de Git, switch en las más recientes)

# merge
Combina los cambios de una rama en otra, generalmente la rama actual.

# diff
Muestra las diferencias entre archivos, ramas, commits, etc.

# log 
Muestra el historial de commits en el repositorio, incluyendo detalles como autor y fecha





## 1. `git clone <url_o_ruta>`  
**Qué hace:**  
Copia todo el repositorio (incluyendo el historial) a tu máquina local.  

**Uso en CTF:**  
Si encuentras una carpeta `.git` accesible en un servidor web, clónala directamente:  
```bash
git clone http://target/.git
````

Así obtienes todos los commits y versiones para buscar credenciales o configuraciones antiguas.



## 2. `git log`

**Qué hace:**  
Muestra el historial de commits (quién, cuándo y qué cambió).

**Uso en CTF:**  
Revisa commits tempranos en busca de contraseñas o tokens que luego fueron “eliminados” pero siguen en el historial.

---

## 3. `git show <commit>`

**Qué hace:**  
Muestra los detalles de un commit concreto (diff y metadatos).

**Uso en CTF:**  
Tras identificar un commit sospechoso con `git log`, ejecuta:

```bash
git show 3f2a1b9
```

para ver las líneas exactas que se añadieron o borraron.

---

## 4. `git diff <rev1> <rev2> [-- <ruta>]`

**Qué hace:**  
Compara dos revisiones, o la revisión actual contra el índice.

**Uso en CTF:**  
Contrasta versiones antiguas de archivos sensibles (`.env`, `config.php`) para extraer credenciales eliminadas.

---

## 5. `git checkout <commit>` o `git checkout <branch>`

**Qué hace:**  
Cambia tu copia de trabajo a otra rama o a un commit específico (estado “detached HEAD”).

**Uso en CTF:**  
Muévete a un punto concreto donde el código tenía más información sensible:

```bash
git checkout 3f2a1b9
```

---

## 6. `git branch` y `git branch -a`

**Qué hace:**  
Lista las ramas locales y, con `-a`, también las remotas.

**Uso en CTF:**  
Busca ramas olvidadas (`dev`, `staging`, `test`) que puedan contener configuraciones de depuración o hardcodes.

---

## 7. `git fsck --full --no-reflogs --unreachable --lost-found`

**Qué hace:**  
Revisa la integridad del repositorio y recupera objetos no referenciados.

**Uso en CTF:**  
Recupera commits u objetos “huérfanos” que fueron borrados pero siguen en la base de datos de Git.

---

## 8. `git reflog`

**Qué hace:**  
Muestra el historial de acciones locales (resets, merges, cambios de HEAD).

**Uso en CTF:**  
Si alguien hizo un `git reset` para ocultar algo, podrás ver la referencia anterior de HEAD y volver a ese estado.

---

## 9. `git grep <patrón> [<revisión>]`

**Qué hace:**  
Busca texto dentro del repositorio, opcionalmente en revisiones antiguas.

**Uso en CTF:**  
Escanea todo el historial por keywords como `password`, `secret`, `token`:

```bash
git grep -n "password" $(git rev-list --all)
```

---

## 10. `git archive --format=tar <branch> [<ruta>]`

**Qué hace:**  
Empaqueta una rama o ruta en un archivo tar/zip.

**Uso en CTF:**  
Extrae rápidamente sólo la carpeta de configuración o directorios relevantes de una rama antigua:

```bash
git archive --format=tar origin/dev config/ | tar -xvf -
```

---

**¿Faltan datos o necesitas ejemplos prácticos de alguno de estos comandos?**