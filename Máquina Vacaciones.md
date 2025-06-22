## Escaneo de puertos

hago ping a la máquina para verificar que tengo conexión
![[Pasted image 20250622202844.png]]

Empiezo el escaneo con nmap para ver que puertos tiene abiertos

Veo que tiene el puerto 22 para ssh y el 80 para http, decido mirar la página web para obtener más información
![[Pasted image 20250622202912.png]]


A simple vista no parece que tenga nada, investigo el código de la página por si hay algo oculto
![[Pasted image 20250622203143.png]]

Consigo dos posibles nombres de usuario, Juan y Camilo, voy a probar estos nombres con el servicio ssh

![[Pasted image 20250622203354.png]]

Utilizo el nombre de camilo para el servicio de ssh con la herramienta de hydra para realizar un ataque de fuerza bruta. 

Obtengo la contraseña del usuario camilo

![[Pasted image 20250622204346.png]]

## Escalada de privilegios

Empiezo a intentar escalar privilegios

![[Pasted image 20250622205134.png]]

![[Pasted image 20250622205154.png]]

No encuentro nada relevante, solo un usuario más llamado pedro.

![[Pasted image 20250622211403.png]]

Como en el mensaje que he encontrado antes mencionaban un correo, voy a ver si hay algo en /var/mail que me pueda servir

![[Pasted image 20250622213945.png]]

En el correo encuentro una contraseña
![[Pasted image 20250622214211.png]]
La pruebo con el usuario juan y consigo el acceso
![[Pasted image 20250622224455.png]]
El usuario juan puede ejecutar ruby como administrador, y según gtfobins si ejecuto lo siguiente:

```
ruby -e 'exec "/bin/sh"'
```

Podré obtener una shell como root

![[Pasted image 20250622225057.png]]

Después de ejecutar el comando obtengo el root