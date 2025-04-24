# Estructura


Está compuesto por módulos



para buscar un  exploit en concreto podemos usar el comando **search**, algunos ejemplos podrian ser los siguientes

`search eternalblue`

`search exploit/windows`

`search type:auxiliary telnet`

Para usar un exploit podremos usar el comando use, algunos ejemplos podrian ser los siguientes:

despues de haber hecho un search los podra aparecer algo similar a esto

msf6 > search eternalblue

`Matching Modules`
================

   #  `Name                                      Disclosure Date  Rank     Check  Description`
   `-  ----                                      ---------------  ----     -----  -----------`
   `0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption`
   `1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution`
   `2  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution`
   `3  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection`
   `4  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution`


`Interact with a module by name or index. For example info 4, use 4 or use exploit/windows/smb/smb_doublepulsar_rce`

`msf6 >``` 
`
`
aqui para seleccionar un exploit podremos hacerlo de la siguiente manera

`msf6 > use 0`
`[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp`
`msf6 exploit(windows/smb/ms17_010_eternalblue) >` 

para volver hacia atras usamos back




cuando seleccionas un exploit puedes ver los payloads compatiles usando  show payloads

añadir un workspace

```
workspace -a {nombredelworkspace}
```

para cambiar de workspace simplemente pondremos 

```
workspace {nombredelworkspace}
```


poner en background una session con `ctrl +z`

listar sessions usando `sessions`

cambiar de session usando `session -i {session}`

