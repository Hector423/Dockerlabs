#  Máquina firsthacking

Primero hago ping a la máquina para comprobar la conexión

![image](https://github.com/user-attachments/assets/48049401-2d3f-4084-a467-8bf2bae3d154)

Hago un primer escaneo simple con nmap

![image](https://github.com/user-attachments/assets/93e5a13d-e07d-42a1-a8ae-618bd9895797)

Puedo ver que hay un servicio ftp, voy a hacer un escaneo más profundo para obtener más información

![image](https://github.com/user-attachments/assets/7b0262a0-d024-40ba-acab-603841611143)

Como veo que no hay más servicios que el de ftp, intento hacer una conexion usando el usuario anonimo de ftp para ver si hay suerte

![image](https://github.com/user-attachments/assets/d8e1520e-26c4-4118-b1b1-bfa28c1ca4f0)

No se puede iniciar sessión con el usuario anonimo, así que busco otra forma.

Abro la consola de metasploitable para buscar si la versión del servicio ftp tiene alguna vulnerabilidad reconocida.

![image](https://github.com/user-attachments/assets/acabe9de-021f-4388-8fad-b46d86461d62)

![image](https://github.com/user-attachments/assets/8a5489bb-b5df-46fb-970f-82d866cc08c8)

Parece que hay una, añado todos los requisitos del exploit para que funcione, solo hace falta poner la ip y puerto

![image](https://github.com/user-attachments/assets/784dd65a-d003-4f32-8c78-6b9e7c4ed88d)

Ejecuto el exploit y me da acceso a una shell con root

![image](https://github.com/user-attachments/assets/d0f12aae-db39-4710-9ace-524b72fe0575)
