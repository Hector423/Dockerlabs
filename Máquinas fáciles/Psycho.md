# Máquina psycho

## Escaneo de puertos

Empiezo realizando un ping para comprobar que tengo conexión con la máquina

![[Pasted image 20250706111723.png]]

Ahora realizo un escaneo con nmap para saber que servicios tiene activos la máquina victima

![[Pasted image 20250706111913.png]]

Como solo tiene dos servicios (ssh y http) decido ir a la página web para ver que hay

## Servicio http

![[Pasted image 20250706112301.png]]

El servicio http solo muestra la página por defecto de apache, así que voy a realizar una enumeración con la herramienta gobuster

![[Pasted image 20250706112846.png]]

Descubro una ruta que se llama assets e index

![[Pasted image 20250706113014.png]]

Veo que solo hay una imagen .jpg, la descargo para ver si contiene algo pero no encuentro nada


![[Pasted image 20250706190003.png]]

En la página de index.php aparece una página simple, voy a seguir inspeccionando el codigo

![[Pasted image 20250706192046.png]]

Lo único destacable es que parece haber un error en la página aunque no se a que se refiere exactamente, puede que sea que este intentando llamar a algo y que no esté bien programado, así que decido utilizar otras herramientas para buscar más información en la página

## Fuzzing web

Decido usar wfuzz para poder encontrar lo que el código de index.php esta llamando pero me da error, aunque después de afinar el comando me sale un parámetro llamado secret
![[Pasted image 20250707114440.png]]

Ahora puedo aprovechar ese parámetro para intentar inyectar valores para ver si puedo aprovecharlo.

Esto es una vulnerabilidad LFI y me permite por ejemplo realizar un Directory Path Traversal, que consiste en "ir hacia atrás" o hacia el directorio raíz para poder leer archivos restringidos 

![[Pasted image 20250707171259.png]]

Como se puede ver me permite leer el archivo /etc/passwd lo que me permite saber que usuarios hay en el sistema. Parece que hay dos usuarios, vaxei y luisillo así que voy a probar a realizar un ataque de fuerza bruta con esos usuarios.

![[Pasted image 20250707173043.png]]
No parece que pueda hacer fuerza bruta, así que me vuelvo a la página web.

Puedo probar a buscar el código fuente de la página web pero para que no me ejecute el código php y pierda información debo usar php://filter que es un wrapper que permite aplicar filtros a php antes de que se procesen, así que esto junto también con esto convert.base64-encode consigo codificarlo a base64 para posteriormente descodificarlo en mi maquina. 

php://filter/convert.base64-encode/resource=index.php

![[Pasted image 20250707174139.png]]

Me genera una cadena en base64 que si la descodifico me sale el código fuente de la página.

![[Pasted image 20250707174249.png]]

Puedo ver la parte de php en la que se llama a secret

![[Pasted image 20250707174319.png]]

Gracias a esto ahora se que no hay ningún tipo de filtro para secret.

## Acceso a la maquina

También puedo provar a leer algun archivo de id_rsa para poder hacer ssh con la clave privada de los usuarios del sistema

![[Pasted image 20250707181128.png]]

Si me copio esta clave y la añado a un archivo puedo iniciar session con el usuario vaxei.

![[Pasted image 20250707181236.png]]

Ya tengo acceso al sistema, ahora empieza la escalada de privilegios

## Escalada de privilegios

![[Pasted image 20250707181427.png]]

El usuario vaxei puede usar perl como el usuario luisillo sin pedir la contraseña, así que puedo obtener una shell como el usuario luisillo.

![[Pasted image 20250707182432.png]]

Ahora ya tengo acceso al usuario luisillo, si ejecuto sudo -l:

![[Pasted image 20250707182701.png]]

El usuario luisillo puede ejecutar como root pyhton3 y paw.py

Voy a ver el contenido del script llamado paw.py

![[Pasted image 20250707184441.png]]

Parece que el script hace mucha cosa, así que voy a ejecutarlo y ver que hace

![[Pasted image 20250707184821.png]]

parece que intenta buscar un archivo llamado 'echo Hello!' y también da error con subprocess.py

![[Pasted image 20250707185439.png]]

Voy a centrarme en el subprocess para ver si puedo crear un código malicioso.

Pyhton funciona de forma que primero consulta de forma local si existen los archivos que importa, por lo tanto puedo aprovechar esto de forma de que puedo crear un archivo en el mismo directorio que paw.py llamado subprocess.py para poder ejecutar código malicioso

En mi archivo subprocess.py hago que ejecute una shell:
![[Pasted image 20250707194404.png]]

Y cuando ejecuto otra vez el código me genera una shell con root

![[Pasted image 20250707194335.png]]

Para entender lo que he hecho (sobre todo para mi), la técnica usada para poder escalar a root se llama Python Module Hijacking, y consiste en crear un modulo de python malicioso para que este ejecute primero el creado por mi ya que python lo que hace es primero consultar de forma local o en la variable de PYTHONPATH las rutas definidas de cada módulo importado. Al añadir mi modulo malicioso en el mismo directorio que paw.py, pyhton ejecutará mi modulo directamente y por eso se ejecuta la shell que obviamente la shell se ejecuta como root ya que el usuario luisillo lo puede ejecutar como root sin necesidad de la contraseña.







