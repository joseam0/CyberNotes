



## Descripción  
Crunch es una herramienta utilizada para generar diccionarios personalizados de contraseñas. Es especialmente útil en pruebas de penetración, donde se requiere realizar ataques de fuerza bruta contra servicios, aplicaciones o dispositivos que utilizan contraseñas como método de autenticación. La fortaleza de Crunch radica en la capacidad de definir patrones específicos, longitudes, combinaciones y caracteres que componen las posibles contraseñas, optimizando los intentos hacia las contraseñas más probables según el contexto.

---

## Funcionalidades y Casuísticas

### 1. **Generación de Diccionarios de Tamaño Personalizado**
   - **Comando:**  
     ```bash
     crunch <min_length> <max_length>
     ```
   - **Ejemplo:**  
     ```bash
     crunch 4 6
     ```
     Genera un diccionario de contraseñas con longitud entre 4 y 6 caracteres, combinando por defecto letras y números.

---

### 2. **Uso de un Conjunto Personalizado de Caracteres**
   - **Comando:**  
     ```bash
     crunch <min_length> <max_length> <charset>
     ```
   - **Ejemplo:**  
     ```bash
     crunch 6 6 abc123
     ```
     Genera contraseñas de 6 caracteres que solo contienen `a`, `b`, `c`, `1`, `2`, y `3`.

---

### 3. **Guardar el Diccionario en un Archivo**
   - **Comando:**  
     ```bash
     crunch <min_length> <max_length> <charset> -o <output_file>
     ```
   - **Ejemplo:**  
     ```bash
     crunch 8 8 abcdefgh -o passwords.txt
     ```
     Guarda todas las combinaciones de 8 caracteres en el archivo `passwords.txt`.

---

### 4. **Patrones Específicos en Contraseñas**
   - **Comando:**  
     ```bash
     crunch <min_length> <max_length> -t <pattern>
     ```
   - **Patrones Comunes:**
     - `@` para letras minúsculas.
     - `,` para letras mayúsculas.
     - `%` para números.
   - **Ejemplo:**  
     ```bash
     crunch 8 8 -t pass%%%%
     ```
     Genera contraseñas de 8 caracteres que empiezan con `pass` seguido de 4 dígitos, como `pass1234`.

---

### 5. **Evitar Combinaciones Repetidas**
   - **Comando:**  
     ```bash
     crunch <min_length> <max_length> <charset> -d <num_repeats>
     ```
   - **Ejemplo:**  
     ```bash
     crunch 4 4 abcd -d 2
     ```
     Genera combinaciones de 4 caracteres sin repetir el mismo carácter más de 2 veces consecutivas.

---

### 6. **Tamaño de Salida Limitado**
   - **Comando:**  
     ```bash
     crunch <min_length> <max_length> <charset> -o START -b <size>
     ```
   - **Ejemplo:**  
     ```bash
     crunch 6 6 abc123 -o passwords.txt -b 10mb
     ```
     Divide la salida en archivos de 10 MB.

---

### 7. **Uso Combinado con Otras Herramientas**
   - **Ejemplo:**  
     Con **Hydra** para un ataque de fuerza bruta:
     ```bash
     crunch 6 6 abc123 | hydra -l admin -P - 192.168.1.1 ssh
     ```
     Aquí, Crunch genera las contraseñas y las pasa directamente como entrada a Hydra para un ataque SSH.

---

## Tabla Resumen

| Función                         | Comando                                              | Ejemplo                                                 |
|---------------------------------|------------------------------------------------------|---------------------------------------------------------|
| Longitud personalizada          | `crunch <min> <max>`                                | `crunch 4 6`                                            |
| Charset definido                | `crunch <min> <max> <charset>`                      | `crunch 6 6 abc123`                                     |
| Guardar en archivo              | `crunch <min> <max> <charset> -o <output_file>`     | `crunch 8 8 abcdefgh -o passwords.txt`                 |
| Uso de patrones                 | `crunch <min> <max> -t <pattern>`                  | `crunch 8 8 -t pass%%%%`                                |
| Limitar repeticiones            | `crunch <min> <max> <charset> -d <num_repeats>`    | `crunch 4 4 abcd -d 2`                                  |
| Dividir en tamaños limitados    | `crunch <min> <max> <charset> -o START -b <size>`   | `crunch 6 6 abc123 -o passwords.txt -b 10mb`           |
| Integración con otras herramientas | `crunch <min> <max> <charset> | tool_command`     | `crunch 6 6 abc123 | hydra -l admin -P - 192.168.1.1 ssh` |

---

Esta herramienta es ideal en escenarios donde se tienen patrones claros de contraseñas a atacar o cuando es necesario optimizar el proceso de fuerza bruta, evitando diccionarios genéricos.
