41-54

codecepticon para ofuscar el codigo fuente

confuserex para ofuscar binarios, es un ofuscador de .NET

netloader para reemplazar llamadas a APIS por versiones ofuscadas

CsWhispers para modificar netloader

Nymcrypt2 para ofuscar el binario resultante


comandos


```
C:\AD\Tools\Codecepticon.exe --action obfuscate --module csharp --verbose - -path "C:\AD\Tools\Rubeus-master\Rubeus.sln" --map-file " C:\AD\Tools\Rubeus-master\Mapping.html" --profile rubeus --rename ncefpavs --rename-method markov --markov-min-length 3 --markov-max-length 10 -- markov-min-words 3 --markov-max-words 5 --string-rewrite --string-rewrite- method xor
```

