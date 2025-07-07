# Máquina Obsession

## Reconocimento

Empiezo realizando un ping a la máquina víctima para saber si tengo conexión.
![Pasted image 20250703110753](https://github.com/user-attachments/assets/995c10ba-e66c-418c-85e9-24c67012d44f)

Hago un escaneo con nmap para poder saber que servicios tiene en los puertos abiertos, puedo comprobar que la máquina tiene: ftp, ssh y http

![Pasted image 20250703110850](https://github.com/user-attachments/assets/6e9d92fe-dea7-4af1-9bd3-81b14bfd08de)

Me doy cuenta de que el servicio de ftp tiene habilitado el inicio de sesión anonimo, y que este tiene permisos de lectura para dos archivos .txt

![Pasted image 20250703111152](https://github.com/user-attachments/assets/99334579-8500-4fc6-88a8-a24d02f599cb)

## Exploración servicio FTP

Sabiendo esto decido empezar primero por el servicio de ftp

![Pasted image 20250703111610](https://github.com/user-attachments/assets/bca614db-3959-4007-a283-0d0001ac47ed)

Efectivamente puedo acceder, voy a leer el contenido de los dos archivos

![Pasted image 20250703111649](https://github.com/user-attachments/assets/8428ced7-5ed5-4b75-93b7-97b889b3a7a4)

De los dos archivos se puede sacar que existen dos usuarios: Gonza y Russoki y que hay algunos permisos mal configurados en el equipo.

## Exploración servicio HTTP

Antes de intentar realizar fuerza bruta con algunos de los dos usuarios, voy a mirar el servicio de http para ver si puedo obtener más información

![Pasted image 20250703111946](https://github.com/user-attachments/assets/f9ceacb1-13ac-4455-b8d4-e09386f81d9b)

El servicio de http solo lleva a la página por defecto de apache, voy a realizar una enumeración de directorios para ver si hay algo oculto.

![Pasted image 20250703112313](https://github.com/user-attachments/assets/6b139014-13e5-4e97-8181-b0065bb8588c)

Consigo sacar dos rutas nuevas ocultas: /backup y /important

![Pasted image 20250703112426](https://github.com/user-attachments/assets/5aa0b0ef-895c-4670-915b-9fd9c567492c)

En el de backup nos confirma la existencia del usuario russoski

![Pasted image 20250703112822](https://github.com/user-attachments/assets/41960533-8d05-4441-8ee6-219db52f4619)

Y en el de important sale un manifiesto hacker

## Ataque de fuerza bruta y acceso ssh

Una vez comprobado todo el servicio de http, decido realizar un ataque de fuerza bruta con el usuario de russoski

![Pasted image 20250703114101](https://github.com/user-attachments/assets/79794d5b-400f-473e-bf39-9a06a31b1aa8)

Obtengo la contraseña del usuario russoski: iloveme

Voy a usar las credenciales para iniciar sesión en el servicio ssh

![Pasted image 20250703114313](https://github.com/user-attachments/assets/858847a5-c3ae-4b4d-822f-c1416b087317)

## Escalada de privilegios

Dentro del sistema puede ver diferentes archivos interesantes

![Pasted image 20250703131811](https://github.com/user-attachments/assets/41173dbe-b5fb-4403-b0d8-120d5cc01793)

Aunque me quedo con que el usuario russoski puede ejecutar /usr/bin/vim como superusuario

![Pasted image 20250703131954](https://github.com/user-attachments/assets/833ddee0-3542-468f-b8b3-cdeed1dbcd6d)

Ejecutando el siguiente código como administrador obtengo el acceso al superusuario

![Pasted image 20250703132849](https://github.com/user-attachments/assets/971f7f01-f58d-4fb2-83b0-4a6c1c7f60a5)
