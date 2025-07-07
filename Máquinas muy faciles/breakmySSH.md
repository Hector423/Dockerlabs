# Máquina breakmySSH

Primero compruebo que tengo conexión a la máquina
![image](https://github.com/user-attachments/assets/4ed82c65-c466-4f37-ba0e-55728302ce19)

Una vez veo que tengo conexión, realizo un escaneo inicial a la máquina víctima

![image](https://github.com/user-attachments/assets/ae5409b8-4f91-40d4-ad22-3b0a3c26fff9)

Veo que solo hay un servicio de ssh, así que para confirmar y poder obtener más información realizo otro escaneo más profundo

![image](https://github.com/user-attachments/assets/7ce52c48-ae33-4b38-9f81-291cbc20f444)

Comprobado que no hay más servicios abiertos, decido intentar una conexión ssh para ver si puedo obtener un poco más de información

![image](https://github.com/user-attachments/assets/a0b57f7e-66fd-4724-92e2-35de9f5cb008)

No encuentro nada relevante así que decido realizar un ataque de fuerza bruta al servicio de ssh. Para facilitar el ataque voy a usar solo el usuario root.
![image](https://github.com/user-attachments/assets/9f42494e-0965-45cf-97b5-e3e7b10b22c9)

Encuentro la contraseña que me permite iniciar session con el usuario root

![image](https://github.com/user-attachments/assets/783c2bdd-9994-4700-a6f0-515146936894)
