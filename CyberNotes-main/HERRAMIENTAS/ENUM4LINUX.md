


| Opción              | Descripción                                           | Ejemplo de Uso                                             |
|---------------------|-------------------------------------------------------|------------------------------------------------------------|
| `-a`                | Realiza un escaneo completo (equivale a -U -G -S -P -s).| `enum4linux -a {ip}`                                       |
| `-U`                | Enumera información sobre usuarios.                   | `enum4linux -U {ip}`                                       |
| `-G`                | Enumera información sobre grupos.                     | `enum4linux -G {ip}`                                       |
| `-S`                | Enumera la información de SMB.                         | `enum4linux -S {ip}`                                       |
| `-P`                | Enumera información sobre impresoras.                 | `enum4linux -P {ip}`                                       |
| `-u`                | Especifica un nombre de usuario.                       | `enum4linux -u username {ip}`                              |
| `-p`                | Especifica una contraseña.                             | `enum4linux -p password {ip}`                              |
| `-w`                | Especifica un archivo de contraseñas.                 | `enum4linux -w /path/to/passwords.txt {ip}`               |
| `-d`                | Realiza un escaneo de SID.                            | `enum4linux -d {ip}`                                       |
| `-i`                | Muestra información adicional sobre usuarios y grupos.| `enum4linux -i {ip}`                                       |
| `-M`                | Muestra la información de máquinas.                   | `enum4linux -M {ip}`                                       |
| `--shares`          | Enumera las comparticiones de SMB.                    | `enum4linux --shares {ip}`                                |
| `--users`           | Enumera los usuarios de SMB.                          | `enum4linux --users {ip}`                                 |
| `--rid-brute`       | Fuerza el escaneo de RID para enumerar usuarios.     | `enum4linux --rid-brute {ip}`                             |
| `--pass-pol`        | Enumera la política de contraseña de la máquina.      | `enum4linux --pass-pol {ip}`                              |
| `--script`          | Ejecuta un script específico después del escaneo.    | `enum4linux --script custom_script.sh {ip}`              |
| `--version`         | Muestra la versión de Enum4linux.                     | `enum4linux --version`                                    |
| `--help`            | Muestra información de ayuda.                         | `enum4linux --help`                                       |
