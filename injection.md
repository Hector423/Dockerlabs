# Máquina Injection

Primero iniciamos el servicio con el script autorun

![Pasted image 20250608120003](https://github.com/user-attachments/assets/09a3449f-8334-4be1-a9ae-53d216328381)

Hago ping a la ip para comprobar que tengo conexión, con esto también puedo intuir gracias al ttl=64 que se trata de una maquina linux.

ping -c 1 172.17.0.2

![Pasted image 20250608120046](https://github.com/user-attachments/assets/2f16a9c2-098a-4054-8936-b4bf766aa5b0)


---

Utilizo nmap para poder escanear los principales puertos y le añado la opción -sV para poder obtener los servicios en cada puerto

nmap -sV -F 172.17.0.2

![Pasted image 20250608124029](https://github.com/user-attachments/assets/991aa811-9c1a-4820-88d6-c4c52ab72b50)


Voy a la pagina web que tiene alojada el servidor

![Pasted image 20250608120728](https://github.com/user-attachments/assets/124970a3-11fe-42f4-84b5-ac52ba22ec7a)



Compruebo que si añado una ' al login, me salta un error de SQL, por lo tanto es vulnerable a un SQLInjection


![Pasted image 20250608120849](https://github.com/user-attachments/assets/1d90e24d-50a4-4bf5-8563-955ec6d9a6ae)



Si intento acceder utilizando SQLInjection

![Pasted image 20250608122047](https://github.com/user-attachments/assets/93ce5a5a-72d1-4fc5-8bea-db0ece8f2317)




Me deja iniciar sesión como Dylan y obtengo su contraseña.


![Pasted image 20250608122157](https://github.com/user-attachments/assets/97de1339-974d-48b7-9a3b-57f506641917)


Voy a probar a iniciar sesión con este usuario y contraseña en el servicio ssh.

![Pasted image 20250608122426](https://github.com/user-attachments/assets/89a78b23-3ddd-476e-8268-9511f6125e89)



Me deja acceder, ahora tengo que escalar privilegios.

Ejecuto find / -perm -u=s -type f 2>/dev/null para poder buscar binarios explotables


![Pasted image 20250608122951](https://github.com/user-attachments/assets/966b977f-a411-4e12-bc67-080948f366a8)


Veo que puedo usar el binario de /usr/bin/env así que lo uso.

Ejecuto env /bin/sh -p para poder obtener una shell como root

![Pasted image 20250608123515](https://github.com/user-attachments/assets/675c6d51-228c-4f06-bfe3-1cbdf9f0f9a6)


