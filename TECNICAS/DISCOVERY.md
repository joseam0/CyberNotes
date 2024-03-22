



- **/robots.txt**: Puede especificar qué partes del sitio no deben ser rastreadas, gracias a este archivo podemos averiguar rutas de la aplicación.

`User-agent: *`
`Allow: /`
`Disallow: /staff-portal`


- **Favicons**: Es el iconito que se muestra en la ventana, muchas veces no se cambia el que viene por defecto, lo cual puede aportarnos información sobre el framework que se ha usado para la pagina
![[Pasted image 20240318202216.png]]

- **sitemap.xml:** se podria decir que es lo contrario al robots.txt, podemos encontrar rutas de la aplicación de la misma manera


```xml
<urlset>
<url>
<loc>http://10.10.25.2/</loc>
<lastmod>2021-07-19T13:07:32+00:00</lastmod>
<priority>1.00</priority>
</url>

<loc>http://10.10.25.2/s3cr3t-area</loc>
<lastmod>2021-07-19T13:07:32+00:00</lastmod>
<priority>0.80</priority>
</url>
</urlset>
```


- **HTTP HEADERS**: al hacer una petición simple a una web `curl http://1.1.1.1 -v` a veces el servidor responde con cabeceras que nos pueden dar información sobre el tipo y version del servidor

 `HTTP/1.1 200 OK`
`< Server: nginx/1.18.0 (Ubuntu)`
`< Date: Mon, 18 Mar 2024 19:21:02 GMT`
`< Content-Type: text/html; charset=UTF-8`
`< Transfer-Encoding: chunked`
`< Connection: keep-alive`
`< X-FLAG: THM{HEADER_FLAG}``
`

- Enumeración de subdominios:
	- Se puede realizar de 3 maneras, por fuerza bruta, por OSINT y por VirtualHosts
		- FUERZA BRUTA: usando herramientas como [[DNSRECON]]
		- OSINT: usando herramientas como [[Sublist3r]]
		- Virtual Hosts: usando herramientas como [[FFUF]]