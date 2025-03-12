59-72

presenta las herramientas que se van a usar para enumerar el AD




## comandos


### Obtener información del dominio usando Powerview


```
Get-Domain
```

con este comando podemos ver la siguiente informacion del dominio en el que nos encontramos
![[Pasted image 20250312225720.png]]

### obtener el identificador de dominio usando powerview

```
Get-DomainSID
```


### obtener informacion del forest root usando powerview
```
Get-Domain -Domain moneycorp.local
```


### obtener información de las policy del dominio  usando powerview

```
Get-DomainPolicyData
```

![[Pasted image 20250312230314.png]]


### obtener informacion de los domain controllers usando powerview

```
Get-DomainController
```

### Listar usuarios del dominio usando powerview

```
Get-DomainUser
```
 este comando nos dara todos los datos de todos los users, para filtrar para que solo salga el  nombre y la cantidad de logins  podriamos añadirle lo siguiente
```
 Get-DomainUser | select samaccountname,logonCount
```

si por ejemplo vemos un logoncount muy bajo o 0 es probable que se sea un honeypot
Si por ejemplo queremos listar los usuarios que contengan un determinado string en sus atributos

```
Get-DomainUser -LDAPFilter "Description=*built*" | Select name,Description
```
el instructor deja caer que a veces se encuentran cosas interesantes en el atributo Description

## Listar computers del dominio usando Powerview

por defecto un usuario de dominio puede añadir hasta 10 objetos computer

```
Get-DomainComputer | select Name
```



### Listar grupos del dominio usando Powerview

```
Get-DomainGroup | select Name
```

tambien podriamos por ejemplo listar todos los grupos que contengan la palabra "admin" 

```
Get-DomainGroup *admin*
```
