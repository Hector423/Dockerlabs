# Máquina psycho

## Escaneo de puertos

Empiezo realizando un ping para comprobar que tengo conexión con la máquina

![Pasted image 20250706111723](https://github.com/user-attachments/assets/b4c6a53d-6072-4caa-ae76-82b8d14c32b8)


Ahora realizo un escaneo con nmap para saber que servicios tiene activos la máquina victima

![Pasted image 20250706111913](https://github.com/user-attachments/assets/358a3eb7-ad44-4e2b-a3b3-6930d7d81045)


Como solo tiene dos servicios (ssh y http) decido ir a la página web para ver que hay

## Servicio http

![Pasted image 20250706112301](https://github.com/user-attachments/assets/3107c60d-2e4d-46c7-af63-1996f8287085)

El servicio http solo muestra la página por defecto de apache, así que voy a realizar una enumeración con la herramienta gobuster

![Pasted image 20250706112846](https://github.com/user-attachments/assets/b437765e-12f8-43c1-96df-ad92fb7013e4)

Descubro una ruta que se llama assets e index

![Pasted image 20250706113014](https://github.com/user-attachments/assets/c90599c0-3b03-4c30-8384-048d8521fccd)


En assets veo que solo hay una imagen .jpg, la descargo para ver si contiene algo pero no encuentro nada

En la página de index.php aparece una página simple, voy a seguir inspeccionando el codigo

![Pasted image 20250706190003](https://github.com/user-attachments/assets/469185ab-fb5c-4b25-8ba3-1be15b63c752)

Lo único destacable es que parece haber un error en la página aunque no se a que se refiere exactamente, puede que sea que este intentando llamar a algo y que no esté bien programado, así que decido utilizar otras herramientas para buscar más información en la página

![Pasted image 20250706192046](https://github.com/user-attachments/assets/d387b92f-1af1-4003-9759-6d1121499d3d)

## Fuzzing web

Decido usar wfuzz para poder encontrar lo que el código de index.php esta llamando pero me da error, aunque después de afinar el comando me sale un parámetro llamado secret

![Pasted image 20250707114440](https://github.com/user-attachments/assets/f758dd2a-6f40-4108-8146-68173b2fc9d5)

Ahora puedo aprovechar ese parámetro para intentar inyectar valores para ver si puedo aprovecharlo.

Esto es una vulnerabilidad LFI y me permite por ejemplo realizar un Directory Path Traversal, que consiste en "ir hacia atrás" o hacia el directorio raíz para poder leer archivos restringidos 

![Pasted image 20250707171259](https://github.com/user-attachments/assets/93663f93-bd67-4db4-a0ae-30feae603e74)


Como se puede ver me permite leer el archivo /etc/passwd lo que me permite saber que usuarios hay en el sistema. Parece que hay dos usuarios, vaxei y luisillo así que voy a probar a realizar un ataque de fuerza bruta con esos usuarios.

![Pasted image 20250707173043](https://github.com/user-attachments/assets/bbb49d0f-6853-4d23-ba8c-6d8b2a75a77e)

No parece que pueda hacer fuerza bruta, así que me vuelvo a la página web.

Puedo probar a buscar el código fuente de la página web pero para que no me ejecute el código php y pierda información debo usar php://filter que es un wrapper que permite aplicar filtros a php antes de que se procesen, así que esto junto también con esto convert.base64-encode consigo codificarlo a base64 para posteriormente descodificarlo en mi maquina. 

php://filter/convert.base64-encode/resource=index.php

![Pasted image 20250707174139](https://github.com/user-attachments/assets/3d100e05-5222-47b4-92cf-3873eb0796cd)

Me genera una cadena en base64 que si la descodifico me sale el código fuente de la página.

![Pasted image 20250707174249](https://github.com/user-attachments/assets/93be75ef-656e-4313-a5f4-07f676c878a8)

Puedo ver la parte de php en la que se llama a secret

![Pasted image 20250707174319](https://github.com/user-attachments/assets/b59e603c-c40c-4741-a18a-97a8a82cd06e)

Gracias a esto ahora se que no hay ningún tipo de filtro para secret.

## Acceso a la maquina

También puedo provar a leer algun archivo de id_rsa para poder hacer ssh con la clave privada de los usuarios del sistema

![Pasted image 20250707181128](https://github.com/user-attachments/assets/394dec83-186f-41d6-ac7a-b51445bb1108)


Si me copio esta clave y la añado a un archivo puedo iniciar session con el usuario vaxei.

![Pasted image 20250707181236](https://github.com/user-attachments/assets/40a20ca7-fede-41a4-ac92-5cf1134cbd4f)

Ya tengo acceso al sistema, ahora empieza la escalada de privilegios

## Escalada de privilegios

![Pasted image 20250707181427](https://github.com/user-attachments/assets/64d49e8a-a066-419b-a994-a3cb212e5d1c)

El usuario vaxei puede usar perl como el usuario luisillo sin pedir la contraseña, así que puedo obtener una shell como el usuario luisillo.

![Pasted image 20250707182432](https://github.com/user-attachments/assets/47af8f7f-42fc-43d4-8ce3-8eb0db6cd1ad)

Ahora ya tengo acceso al usuario luisillo, si ejecuto sudo -l:

![Pasted image 20250707182701](https://github.com/user-attachments/assets/bba9cd49-32c5-4e99-8574-5a42b2a902b2)

El usuario luisillo puede ejecutar como root pyhton3 y paw.py

Voy a ver el contenido del script llamado paw.py

![Pasted image 20250707184441](https://github.com/user-attachments/assets/d291fff8-7da4-41f2-b3a2-5285dba3da58)

Parece que el script hace mucha cosa, así que voy a ejecutarlo y ver que hace

![Pasted image 20250707184821](https://github.com/user-attachments/assets/3e33aa01-21b7-4c9a-9d84-6a53356eaf97)

parece que intenta buscar un archivo llamado 'echo Hello!' y también da error con subprocess.py

![Pasted image 20250707185439](https://github.com/user-attachments/assets/39de9627-6c81-4f5d-bc98-a23f54afa1cb)

Voy a centrarme en el subprocess para ver si puedo crear un código malicioso.

Pyhton funciona de forma que primero consulta de forma local si existen los archivos que importa, por lo tanto puedo aprovechar esto de forma de que puedo crear un archivo en el mismo directorio que paw.py llamado subprocess.py para poder ejecutar código malicioso

En mi archivo subprocess.py hago que ejecute una shell:

![Pasted image 20250707194404](https://github.com/user-attachments/assets/50035148-6398-408a-a590-eb469201db20)

Y cuando ejecuto otra vez el código me genera una shell con root

![Pasted image 20250707194335](https://github.com/user-attachments/assets/ef486313-6e97-4da2-9a0e-d6c97a1653cb)

Para entender lo que he hecho (sobre todo para mi), la técnica usada para poder escalar a root se llama Python Module Hijacking, y consiste en crear un modulo de python malicioso para que este ejecute primero el creado por mi ya que python lo que hace es primero consultar de forma local o en la variable de PYTHONPATH las rutas definidas de cada módulo importado. Al añadir mi modulo malicioso en el mismo directorio que paw.py, pyhton ejecutará mi modulo directamente y por eso se ejecuta la shell que obviamente la shell se ejecuta como root ya que el usuario luisillo lo puede ejecutar como root sin necesidad de la contraseña.







