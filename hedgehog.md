# Máquina hedgehog

Primero compruebo la conexión a la máquina

![image](https://github.com/user-attachments/assets/34abf86a-6948-4f0f-9b1f-28015cb1ac34)

Una vez revisado que tengo conexión, procedo a hacer un escaneo con nmap

![image](https://github.com/user-attachments/assets/72529455-c50f-46ae-836a-6656d2e9dc66)

El escaneo me muestra dos servicios, ssh y apache en los puertos habituales.
Empiezo a hacer enumeración con gobuster para ver que puedo obtener

![image](https://github.com/user-attachments/assets/d0799e3f-ba46-4906-8599-660519037f5a)

No me muestra nada que pueda utilizar, voy a la propia página para ver que contiene

![image](https://github.com/user-attachments/assets/93616800-96cf-416d-a346-bfd454ab56ed)

La página solo muestra la palabra tails, que si no me equivoco podría ser el nombre de Tails de la saga de Sonic.

Pruebo a hacer fuerza bruta al servicio ssh con el nombre de tails para ver si puedo sacar la contraseña.

![image](https://github.com/user-attachments/assets/e17e90ea-856e-4df4-bc5e-81fc3eae93a9)


Despúes de dejar hydra un rato no consigo sacar nada, pero se me ocurre que tails también se puede referir a el comando tail, que sirve para ver el final de un archivo,
asi que lo que hago es copiar las últimas 100 lineas del diccionario rockyou para ver si la contraseña esta en el final.
![image](https://github.com/user-attachments/assets/a35fefa6-387a-4df5-bbec-75f02342e804)

También será necessario quitar los espacios, para ello.
![image](https://github.com/user-attachments/assets/ee0c1f6a-4409-4631-9287-7f0b835f7de7)

Y si ahora ejecuto hydra

![image](https://github.com/user-attachments/assets/28bf71cd-6bcd-4f92-a928-fd9523192752)

Me encuentra la contraseña, así que hago login en ssh

![image](https://github.com/user-attachments/assets/467b882e-f632-4439-8e13-6a6f7ca11efd)

Una vez dentro empieza la escalada de privilegios

# Escalada de privilegios

Primero busco binarios

![image](https://github.com/user-attachments/assets/eb285bbb-ef60-453f-8dbe-c8618658daf8)

![image](https://github.com/user-attachments/assets/cd3a8366-6ac9-47ad-8d66-1d278b6c04a6)

Al ejecutar sudo -l veo que puedo llegar hasta el usuario sonic sin necesidad de la contraseña

![image](https://github.com/user-attachments/assets/a33b274f-08e9-49fd-a061-d89f4bc69ee8)

Ahora tengo que ver que puede hacer sonic

![image](https://github.com/user-attachments/assets/c42d69ec-8c0f-48c1-a479-3fd5b519159f)

Parece ser que sonic puede llegar hasta root sin necesidad de contraseña, así que ejecuto sudo su

![image](https://github.com/user-attachments/assets/30a586db-43fc-4370-bf35-d0facf16b2e3)

Y obtengo acceso al usuario root.
