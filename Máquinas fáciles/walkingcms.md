
# Enumeración

### Nmap

Primero realizo un escaneo inicial para ver los puertos abiertos
asd

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250921133901.png>)

Veo que hay un servicio de Apache en el puerto 80, realizo otro escaneo con nmap utilizando scripts básicos para obtener más información

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250921134622.png>)

No obtengo mucha más información así que realizo enumeración utilizando dirbuster

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250921135238.png>)
A medida que avanza el escaneo puedo ver diferentes rutas, voy a ver la de wordpress

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250921135324.png>)
Se puede ver una página por defecto de wordpress así que eso me hace pensar que alomejor el login para el panel de admin es el que hay por defecto

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250921135542.png>)

efectivamente puedo acceder, como veo que el escaneo no encuentra nada más, procedo a realizar fuerza bruta sobre el login con wpscan

# Explotación 

### Enumeración

Con wpscan podemos realizar una pequeña enumeracion para intentar encontrar usuarios, utilizando el comando 

`wpscan --url http://172.17.0.2/wordpress/ --enumerate u,vp`

Con este comando realizo una enumeración de usuarios y de pluguins vulnerables

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922162203.png>)
Como se puede ver no encuentra ningun pluguin vulnerable

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922162242.png>)

Pero si que detecta un usuario que se llama mario, esto me permite ahora realizar un ataque de fuerza bruta usando la misma herramienta
### Fuerza bruta

`wpscan -U mario -P SecLists/Passwords/Common-Credentials/10k-most-common.txt --url http://172.17.0.2/wordpress/wp-login.php`

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922162537.png>)

Consigo su contraseña: love

Inicio sessión y ahora que solo obtener una shell remota, para eso lo que puedo hacer es modificar uno de los temas que se esten usando para poder modificar su php y así inyectar una reverse shell

Primero tengo que ver que tema se esta usando

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922170448.png>)

El tema que se esta usando es el de twenty twenty-two

Ahora voy a theme code editor y busco cualquier archivo php para editarlo

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922171007.png>)

Voy a la página de revshell y genero mi codigo

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922171111.png>)

Copio y pego todo el contenido php al archivo php de la página y lo actualizo

En un terminal de mi maquina ejecuto 

`nc -lnvp 1234`

Ahora solo queda ir a la ruta donde esta la página

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922171304.png>)

Y una vez dentro me llega la shell

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922171420.png>)

# Escalada

### Mejorar shell


`script /dev/null -c bash`
`Cntrl + z`
`stty raw -echo; fg`
`reset xterm`
`export TERM=xterm`

Con esto ya debería bastar para la shell

### Escalada de privilegios

Voy a empezar a ver como puedo escalar privilegios, primero ejecuto `sudo -l`

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922173803.png>)
Me da error así que tengo que buscar otra forma, voy a mirar binarios SUID 

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922173846.png>)

De entre todos ellos, el binario env me puede servir para ralizar una escalada, si miro la página gtfobins me indica lo que tengo que hacer

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922173956.png>)

![](<../Imagenes Dockerlabs/Imagenes Walkingcms/Pasted image 20250922174051.png>)

Obtengo acceso a root

