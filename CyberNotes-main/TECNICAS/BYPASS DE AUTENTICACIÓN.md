

Enumeración de usuarios:


muchas veces a la hora de registrarnos justo al completar el proceso nos indica la pagina que el usuario ya existee, por lo que una manera fácil de enumerar usuarios podria ser haciendo fuzzing con las peticiones de registro y filtrando en la respuesta el error del tipo "este usuario ya esta en uso".
Esto lo podremos hacer con herramientas como burpsuite o [[ffuf]]