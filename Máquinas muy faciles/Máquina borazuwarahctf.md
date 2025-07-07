
# Máquina borazuwarahctf

Primero de todo empiezo haciendo un ping para comprobar si tengo conexión con la máquina

![Pasted image 20250618120834](https://github.com/user-attachments/assets/eae4fc62-ba2c-4296-b789-e57e7d9e6f3f)

Después hago un escaneo de puertos con nmap

![Pasted image 20250618120819](https://github.com/user-attachments/assets/f23767f9-a604-4698-8613-6ce998fbd6d4)

El escaneo me muestra dos servicios, un servicio de ssh en el puerto 22 y uno de http en el 80.

Decido ir a la página web para poder obtener más información

![Pasted image 20250618121258](https://github.com/user-attachments/assets/ead6f7c1-8840-4ccc-ad4d-3a8b49036029)

En la página solo hay una imagen de un huevo kinder así que decido descargar la imagen para ver si tiene dentro información escondida

![Pasted image 20250618121848](https://github.com/user-attachments/assets/0f50e150-8242-4bd3-aef0-32e41f61e448)


Uso la herramiento exiftool y encuentro un usuario: borazuwarah. Aunque no contiene la contraseña así que voy a tener que utilizar fuerza bruta.

![Pasted image 20250618122027](https://github.com/user-attachments/assets/4418e67a-686a-4787-b21e-e2598872e2e5)

Con hydra simplemente añado el usuario encontrado y utilizo el diccionario rockyou

![Pasted image 20250618122244](https://github.com/user-attachments/assets/ca90fd35-77e0-4ee6-aa93-e75c70c5260d)

Consigo iniciar sessión, ahora solo queda escalar privilegios

![Pasted image 20250618122352](https://github.com/user-attachments/assets/f88e5951-1fef-4713-b004-2dd9734439d2)

# Escalada de privilegios

Empiezo buscando possibles formas para poder escalar privilegios, primero ejecuto sudo -l

![Pasted image 20250618122448](https://github.com/user-attachments/assets/803a0ae6-9236-4aa0-8863-eee3cff5740b)


Y me doy cuenta de que el usuario borazuwarah puede escalar a root, así que simplemente ejecuto sudo su

![Pasted image 20250618122642](https://github.com/user-attachments/assets/11675088-9cac-4046-8eef-84b9158cf300)

Y ya soy root en el sistema.






