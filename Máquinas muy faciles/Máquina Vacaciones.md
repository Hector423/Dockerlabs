## Escaneo de puertos

hago ping a la máquina para verificar que tengo conexión
![Pasted image 20250622202844](https://github.com/user-attachments/assets/401b4f9b-2c70-4376-acd2-7900ff2fc369)


Empiezo el escaneo con nmap para ver que puertos tiene abiertos

Veo que tiene el puerto 22 para ssh y el 80 para http, decido mirar la página web para obtener más información
![Pasted image 20250622202912](https://github.com/user-attachments/assets/d7e6edcc-db65-4c16-8c86-e2d8274947e5)



A simple vista no parece que tenga nada, investigo el código de la página por si hay algo oculto
![Pasted image 20250622203143](https://github.com/user-attachments/assets/238b41cc-028b-4238-9870-a3021194d9dd)


Consigo dos posibles nombres de usuario, Juan y Camilo, voy a probar estos nombres con el servicio ssh
![Pasted image 20250622203354](https://github.com/user-attachments/assets/cf14368f-66e2-44e9-b9f5-f7d693fe818c)



Utilizo el nombre de camilo para el servicio de ssh con la herramienta de hydra para realizar un ataque de fuerza bruta. 

Obtengo la contraseña del usuario camilo

![Pasted image 20250622204346](https://github.com/user-attachments/assets/f2857706-c04c-4c62-9c1b-2ac931d35e13)


## Escalada de privilegios

Empiezo a intentar escalar privilegios

![Pasted image 20250622205134](https://github.com/user-attachments/assets/af94aed1-24b7-48dc-84cf-b70738484d55)
![Pasted image 20250622205154](https://github.com/user-attachments/assets/45f2c824-2481-462b-9381-1db390a318d7)


No encuentro nada relevante, solo un usuario más llamado pedro.

![Pasted image 20250622211403](https://github.com/user-attachments/assets/ffa76692-1e7e-4de7-b889-618718c02cf8)

Como en el mensaje que he encontrado antes mencionaban un correo, voy a ver si hay algo en /var/mail que me pueda servir

![Pasted image 20250622213945](https://github.com/user-attachments/assets/48df8c20-6bfc-41b9-a55a-9d4e8529611a)

En el correo encuentro una contraseña

![Pasted image 20250622214211](https://github.com/user-attachments/assets/e5a8ad9a-76df-4072-988a-ade2fdb81516)

La pruebo con el usuario juan y consigo el acceso

![Pasted image 20250622224455](https://github.com/user-attachments/assets/23209d6e-6506-4fd2-af42-55882e2a31bd)

El usuario juan puede ejecutar ruby como administrador, y según gtfobins si ejecuto lo siguiente:

![Pasted image 20250622230328](https://github.com/user-attachments/assets/6f008138-a269-4809-9235-b7734a99533a)

```
ruby -e 'exec "/bin/sh"'
```

Podré obtener una shell como root
![Pasted image 20250622225057](https://github.com/user-attachments/assets/c6bf946f-1287-46fb-9145-3be921ca76dc)

Después de ejecutar el comando obtengo el root
