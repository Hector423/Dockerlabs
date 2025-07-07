
![[Pasted image 20250703110753.png]]
Empiezo realizando un ping a la máquina víctima para saber si tengo conexión.

![[Pasted image 20250703110850.png]]

Hago un escaneo con nmap para poder saber que servicios tiene en los puertos abiertos, puedo comprobar que la máquina tiene: ftp, ssh y http

Me doy cuenta de que el servicio de ftp tiene habilitado el inicio de sesión anonimo, y que este tiene permisos de lectura para dos archivos .txt
![[Pasted image 20250703111152.png]]

Sabiendo esto decido empezar primero por el servicio de ftp

![[Pasted image 20250703111610.png]]
Efectivamente puedo acceder, voy a leer el contenido de los dos archivos

![[Pasted image 20250703111649.png]]

De los dos archivos se puede sacar que existen dos usuarios: Gonza y Russoki y que hay algunos permisos mal configurados en el equipo.

Antes de intentar realizar fuerza bruta con algunos de los dos usuarios, voy a mirar el servicio de http para ver si puedo obtener más información

![[Pasted image 20250703111946.png]]

El servicio de http solo lleva a la página por defecto de apache, voy a realizar una enumeración de directorios para ver si hay algo oculto.

![[Pasted image 20250703112313.png]]

Consigo sacar dos rutas nuevas ocultas: /backup y /important

![[Pasted image 20250703112426.png]]
En el de backup nos confirma la existencia del usuario russoski

![[Pasted image 20250703112822.png]]
Y en el de important sale un manifiesto hacker

Una vez comprobado todo el servicio de http, decido realizar un ataque de fuerza bruta con el usuario de russoski

![[Pasted image 20250703114101.png]]

Obtengo la contraseña del usuario russoski: iloveme

Voy a usar las credenciales para iniciar sesión en el servicio ssh

![[Pasted image 20250703114313.png]]

Dentro del sistema puede ver diferentes archivos interesantes

![[Pasted image 20250703131954.png]]
Aunque me quedo con que el usuario russoski puede ejecutar /usr/bin/vim como superusuario

![[Pasted image 20250703132849.png]]
ejecutando el siguiente código como administrador obtengo el acceso al superusuario
