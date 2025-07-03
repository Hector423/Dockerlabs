
![[Pasted image 20250702113806.png]]

Primero compruebo si tengo conexión a la máquina victima

![[Pasted image 20250702113850.png]]
Realizo un primer escaneo para poder ver que servicios hay activos en la máquina

![[Pasted image 20250702115231.png]]

![[Pasted image 20250702115623.png]]
Hago un escaneo de la página con gobuster, no encuentro nada

Decido iniciar la msfconsole para poder buscar más cómodo algún exploit o escáner sobre los servicios que tengo disponibles en la máquina

![[Pasted image 20250702122048.png]]

Creo mi espacio de trabajo, asigno la ip de la máquina víctima como global para ahorrar tiempo y realizo otra vez el escaneo de nmap para tener los datos guardados en metasploit.

![[Pasted image 20250702122220.png]]

Ahora voy a buscar escaneos o exploits que me puedan servir por la versión de los servicios

De apache no encuentro nada pero de vsftpd si

![[Pasted image 20250702123026.png]]

Hay un exploit de puerta trasera para la versión que se esta ejecutando.

Lo uso y miro lo que pide para poder ejecutarlo

![[Pasted image 20250702123122.png]]

Ejecuto y obtengo root

![[Pasted image 20250702123309.png]]

Ahora puedo tratar de mejorar la shell para finalizar

![[Pasted image 20250702124426.png]]











