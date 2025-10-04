
# Enumeración

### Nmap

Realizo un primer escaneo

`nmap -p- -sV -sC --open 172.17.0.2 -oN primer_scan.txt`

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923110351.png>)

Hay dos puertos abiertos, el 22 con ssh y el 80 con un servicio web.

Voy a realizar una enumeración con dirbuster en el servicio web

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923110647.png>)

El escaneo encuentra esto, no hay mucha cosa así que voy a mirar que hay en cada una

en index.php me encuentro con un login

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923110922.png>)

En page.php es una pagina para consultar el clima

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923111016.png>)
Y en auth.php parece que da un error al entrar

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923111114.png>)
Parece ser que necesita un usuario y contraseña para que funcione este php

Viendo todo esto lo primero que se me ocurre hacer es comprovar si existe alguna vulnerabilidad de sql injection

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923111508.png>)
Parece que si

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923111528.png>)

Probando diferentes formas de inyección sql doy con una que parece que funciona, aunque el login de la página solo lleva al archivo page.php.

No hay mucho más que pueda hacer de forma manual así que decido utilizar sqlmap para ver si puedo sacar algo.

Copio la petición del login y la pego en un txt

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923171851.png>)

Y lo uso para intentar sacar las bases de datos

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923172036.png>)

Parece que saco una tabla con usuarios y contraseñas

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250923172120.png>)

No parece que pueda acceder con esos usuarios por ssh, pero viendo que un usuario se llama directorio y su contraseña es directoriotravieso puede que sea una pista para encontrar un directorio oculto dentro de el servicio http

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20250927195246.png>)

En efecto descubro un directorio oculto utilizando la contraseña del usuario directorio.

Dentro parece que hay una imagen que se titula miramebien, la voy a descargar y intentar ver si hay algo oculto dentro

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20251004101610.png>)
Con exiftool con consigo encontrar nada

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20251004101657.png>)
Pero usando stegseek consigo encontrar una contraseña y extraer algo oculto en la imagen


Como lo que he extraído es un zip y este pide contraseña también tengo que hacer fuerza bruta, en este caso usaré la herramienta john

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20251004102608.png>)
Ahora ya puedo leer el txt

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20251004102752.png>)

Obtengo un usuario y contraseña

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20251004102904.png>)
Una vez dentro ahora solo queda escalar privilegios

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20251004104223.png>)
Viendo los binarios veo que puedo usar el de find, ya que si miramos en gtfobins vemos que podemos escalar con este comando: `find . -exec /bin/sh -p \; -quit`

![](<../Imagenes Dockerlabs/Imagenes mirame/Pasted image 20251004104404.png>)

