

una vez tenemos una dummy shell con los siguientes comandos la hacemos completamente interactiva

`python3 -c 'import pty;pty.spawn("/bin/bash")’`  
`export TERM=xterm`  
  
`press Ctrl + Z`  
  
`stty raw -echo; fg`