Hay varias versiones de meterpreter


| comando       | utilidad                                                                                                                                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| getuid        | nos dice el usuario que esta corriendo el meterpreter                                                                                                                                                              |
| ps            | lista los procesos en ejecucion, nos puede servir para obtener otro pid al que migrar el proceso                                                                                                                   |
| migrate {pid} | nos permite igrar el meterpreter a otro proceso, de esta manera podremos usar el meterpreter de keylogger(prestar atencion ya que podemos perder privilegios si el proceso lo inicio otro user menos privilegiado) |
| hashdump      | dumpea la SAM y obtenemos los hashes NTLM                                                                                                                                                                          |
| search        | sirve para buscar ficheros                                                                                                                                                                                         |
| load {module} | sirve para cargar modulos que nos aportan nuevos comandos, los mas tipicos son kiwi,powershell,priv                                                                                                                |
