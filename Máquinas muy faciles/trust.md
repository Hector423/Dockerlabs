# Máquina Trust

Inicio la máquina con el autodeploy

![Pasted image 20250609122123](https://github.com/user-attachments/assets/4da35700-96ee-40fa-b09c-7159d1aea9fa)

Realizo un escaneo para saber que servicios hay en los puertos abiertos

![Pasted image 20250609122243](https://github.com/user-attachments/assets/0b752a54-6bc6-486e-b79c-7608ccba0f1b)

Con gobuster realizo una enumeración de los possibles sitios de la web 

![Pasted image 20250609123711](https://github.com/user-attachments/assets/fc565f86-a90f-483a-9885-57cf3a1668a6)

No encuentro nada que me pueda ayudar, voy a la propia web para ver si puedo obtener información desde allí

![Pasted image 20250609124502](https://github.com/user-attachments/assets/d7aa9c93-3440-4e75-8aa8-8256183df321)


La web en si es la página por defecto de apache, no me sirve de mucho, vuelvo a analizar con gobuster para ver si se me escapa algo. Esta vez, pruebo con un diccionario más extenso y también pruebo a buscar diferentes archivos, el resultado me muestra un archivo que se llama secret.php

![Pasted image 20250609141541](https://github.com/user-attachments/assets/4de8c83a-ca77-4607-8eb4-127e843855f9)

Si accedo, me muestra un mensaje diciendo "Hola Mario, esta web no se puede hackear"

![Pasted image 20250609141816](https://github.com/user-attachments/assets/86b6c7e4-e6d5-4948-b590-373be8f7381c)

Si inspecciono la página tampoco me muestra mucho

![Pasted image 20250609142059](https://github.com/user-attachments/assets/e256aa89-fb92-4163-b24b-6f309cc75af7)
![Pasted image 20250609142115](https://github.com/user-attachments/assets/51a02ffc-741d-4f90-ab85-33c89b6bce4e)

Me quedo sin opciones en el puerto 80, decido ir al puerto 22 con el servicio ssh para intentar hacer fuerza bruta para ver que sale de ahí.

Añado el usuario de mario a la búsqueda para facilitar la fuerza bruta y consigo una contraseña. 

![Pasted image 20250609144944](https://github.com/user-attachments/assets/68a6d121-c7b2-4a24-a805-e1f14f3dc065)

Inicio sessión con el usuario en ssh y empiezo a intentar escalar privilegios.

![Pasted image 20250609145221](https://github.com/user-attachments/assets/9d2f2167-9eaf-482d-80fe-8f5e9bc8bd9b)

Encuentro el que usuario mario puede usar vim como root

![Pasted image 20250609145521](https://github.com/user-attachments/assets/3ab412b7-b905-424c-b4cb-c64c65061059)

Y usando lo encontrado en la página gtfobins puedo escalar a root

![Pasted image 20250609145759](https://github.com/user-attachments/assets/268dd843-f08f-4ea7-bcbc-18b152558f40)








