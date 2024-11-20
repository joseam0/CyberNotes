# Enumeración de Usuarios y Contraseñas

1. **Atención en la Página de Registro**  
   - Observar los mensajes proporcionados, como:  
     - "Este usuario ya existe."  
     - "La contraseña debe ser superior a 8 caracteres y contener una letra mayúscula."

2. **Sección "Olvidé mi Contraseña"**  
   - Analizar los mensajes y flujos que podrían revelar información sobre la existencia de usuarios.

3. **Mensajes de Error al Intentar Iniciar Sesión**  
   - Revisar respuestas del sistema para detectar patrones o información útil.

4. **Brechas de Seguridad Pasadas**  
   - Investigar incidentes previos relacionados con el objetivo que puedan dar pistas sobre vulnerabilidades.

---

# Verbose Errors

A veces, las páginas web pueden devolver mensajes de error que exponen información sensible, como:  
- Detalles sobre la base de datos.  
- Información del servidor utilizado.  
- Rutas internas del sistema.  
- Usuarios existentes.  

## ¿Cómo Causar Estos Errores?

1. **Formulario de Login**  
   - Introducir datos no válidos para provocar errores.  

2. **SQL Injection**  
   - Probar con entradas como `'` o `"` en los campos.

3. **Path Traversal**  
   - Usar patrones como `../../../` para intentar acceder a rutas internas.

---

# Enumeración Usando Wayback Machine

- La herramienta necesaria se encuentra en el siguiente repositorio:  
  [https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls)

### Comandos:

```bash
sudo apt install golang-go -y
go build
./waybackurls {objetivo}
