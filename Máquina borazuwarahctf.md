
Primero de todo empiezo haciendo un ping para comprobar si tengo conexión con la máquina

![[Pasted image 20250618120834.png]]

Después hago un escaneo de puertos con nmap

![[Pasted image 20250618120819.png]]

El escaneo me muestra dos servicios, un servicio de ssh en el puerto 22 y uno de http en el 80.

Decido ir a la página web para poder obtener más información

![[Pasted image 20250618121258.png]]

En la página solo hay una imagen de un huevo kinder así que decido descargar la imagen para ver si tiene dentro información escondida

![[Pasted image 20250618121848.png]]

Uso la herramiento exiftool y encuentro un usuario: borazuwarah. Aunque no contiene la contraseña así que voy a tener que utilizar fuerza bruta.

![[Pasted image 20250618122027.png]]

Con hydra simplemente añado el usuario encontrado y utilizo el diccionario rockyou

![[Pasted image 20250618122244.png]]

Consigo iniciar sessión, ahora solo queda escalar privilegios

![[Pasted image 20250618122352.png]]

# Escalada de privilegios

Empiezo buscando possibles formas para poder escalar privilegios, primero ejecuto sudo -l

![[Pasted image 20250618122448.png]]
Y me doy cuenta de que el usuario borazuwarah puede escalar a root, así que simplemente ejecuto sudo su

![[Pasted image 20250618122642.png]]
Y ya soy root en el sistema.






