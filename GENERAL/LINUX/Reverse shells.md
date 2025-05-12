
## Shell interactiva

se crea pseudo terminal con  
```
python -c 'import pty; pty.spawn("/bin/bash")
```

`
 Poner el terminal en modo “raw” y desactivar el eco con 

```
 stty raw -echo
```

Volver al proceso en foreground
```
fg
```

si se esta en una zsh los dos pasos anteriores se hacen en la misma linea con 
```
stty raw -echo; fg

```

se resetea el entorno con
```
reset
```

se exportan las variables necesarias
```
export SHELL=bash
```

```

export TERM=xterm-256color  
```
se ajusta el tamaño de la terminal con 
```
stty rows 10 columns 20
```

