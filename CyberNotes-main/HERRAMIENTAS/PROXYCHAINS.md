
herramienta que permite enrutar las conexiones de red a través de múltiples proxies

se instala con el siguiente comando

`sudo apt-get install proxychains
`
funciona con el archivo de configuración  `proxychains.conf` (ubicado generalmente en `/etc/proxychains.conf`)

un ejemplo de ese archivo seria el siguiente



```python


# Dynamic - Load the proxy list from the file specified
dynamic_chain

# Some timeout settings
tcp_read_time_out 15000
tcp_connect_time_out 8000

# ProxyList format
#       type  host  port  [user pass]
#
# Examples:
# socks5  127.0.0.1 1080    username:password
# http    192.168.1.1 8080  just_username
# socks4  10.0.0.1 1080     username:password@myproxy.example.com

# The following lines configure ProxyChains to use a SOCKS5 proxy with authentication
[ProxyList]
socks5 127.0.0.1 1080 usuario:contraseña

# Additional proxies can be added below
#socks4 10.0.0.1 1080
#http 192.168.1.1 8080


```






`
