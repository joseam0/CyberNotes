

Todo proceso de autenticacion en AD se hace mediante tickets
todas las queries

los tickets se guardan siempre en memoria, nunca en el discoi

# Proceso de Autenticación Kerberos en Active Directory

A continuación se detalla paso a paso el flujo de autenticación Kerberos que aparece en la imagen:

1. **Solicitud de TGT**  
   - El cliente (usuario o equipo) envía al Controlador de Dominio (KDC) su nombre de usuario junto con un sello de tiempo, todo ello cifrado con su clave derivada de la contraseña.  
   - El KDC descifra el mensaje y comprueba que el usuario exista y que el sello de tiempo sea válido.

2. **Emisión del TGT**  
   - Si la autenticación inicial es correcta, el KDC genera un **Ticket Granting Ticket (TGT)**, lo firma y cifra con la clave secreta del servicio `krbtgt`.  
   - A continuación envía este TGT al cliente.

3. **Almacenamiento del TGT**  
   - El cliente recibe el TGT y lo guarda en memoria (en el proceso LSASS), listo para futuras peticiones de tickets de servicio.

4. **Solicitud de Ticket de Servicio (TGS)**  
   - Cuando el cliente necesita acceder a un recurso concreto (por ejemplo, un servidor de base de datos), envía al KDC una petición de **Ticket de Servicio (TGS)**, presentando el TGT anterior.

5. **Emisión del TGS**  
   - El KDC verifica que el TGT esté vigente y, si todo es correcto, genera un **Service Ticket (TGS)** cifrado con la clave del servicio destino (p. ej. `cifs/DBServer`).

6. **Presentación del TGS al servicio**  
   - El cliente envía el TGS al servicio destino (el servidor de base de datos).  
   - El servicio descifra el ticket con su propia clave y extrae el **token de acceso** (que contiene SIDs de usuario, grupos y privilegios).

7. **Autorización y acceso**  
   - Con el token de acceso, el servicio compara los SIDs contra su ACL interna y decide si permite o niega la petición.  
   - Opcionalmente, puede realizar validaciones adicionales contra el KDC (paso «Other optional validation requests»).

---

**Resumen del flujo**:

```mermaid
sequenceDiagram
    actor Cliente
    participant KDC as Controlador de Dominio
    participant Servicio as Servidor de Base de Datos

    Cliente->>KDC: 1. pet. TGT (Usuario + timestamp cifrado)
    KDC-->>Cliente: 2. TGT firmado y cifrado
    Cliente->>KDC: 4. pet. TGS (presentando TGT)
    KDC-->>Cliente: 5. Service Ticket (TGS)
    Cliente->>Servicio: 6. presenta TGS
    Servicio->>Servicio: 7. genera token y comprueba ACL
    note right of Servicio: 8. validaciones opcionales con KDC


![[Pasted image 20250522004913.png]]