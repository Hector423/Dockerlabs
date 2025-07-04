# Máquina tproot

## Reconocimiento

Primero compruebo si tengo conexión a la máquina victima
![Pasted image 20250702113806](https://github.com/user-attachments/assets/9fdc4ddf-85ff-4ef4-8fdc-fa94dfa0efa5)

Realizo un primer escaneo para poder ver que servicios hay activos en la máquina

![Pasted image 20250702113850](https://github.com/user-attachments/assets/8216a4f8-10ae-43a4-b846-777b8831ada5)

Hago un escaneo de la página con gobuster, no encuentro nada

![Pasted image 20250702115231](https://github.com/user-attachments/assets/fb1d1d85-b096-4972-a3bc-75b677ef50a6)

![Pasted image 20250702115623](https://github.com/user-attachments/assets/64f39dd8-3809-48e2-9544-19d5d8dff094)

Decido iniciar la msfconsole para poder buscar más cómodo algún exploit o escáner sobre los servicios que tengo disponibles en la máquina

Creo mi espacio de trabajo, asigno la ip de la máquina víctima como global para ahorrar tiempo y realizo otra vez el escaneo de nmap para tener los datos guardados en metasploit.

![Pasted image 20250702122048](https://github.com/user-attachments/assets/56d87c2f-fb34-464e-ae48-d2ec88b1897b)

![Pasted image 20250702122220](https://github.com/user-attachments/assets/05414e59-e6e0-4676-bc2b-f48cea69e86a)

Ahora voy a buscar escaneos o exploits que me puedan servir por la versión de los servicios

De apache no encuentro nada pero de vsftpd si

![Pasted image 20250702123026](https://github.com/user-attachments/assets/4d065d60-2a85-4478-8e95-94d56b45caa9)

## Explotación y escalada de privilegios

Hay un exploit de puerta trasera para la versión que se esta ejecutando.

Lo uso y miro lo que pide para poder ejecutarlo

![Pasted image 20250702123122](https://github.com/user-attachments/assets/d31376a8-e569-4fd0-8cd5-a84c3be73fa1)

Ejecuto y obtengo root

![Pasted image 20250702123309](https://github.com/user-attachments/assets/6fe55bb9-0476-4ded-9d46-88ee564ebe26)

Ahora puedo tratar de mejorar la shell para finalizar

![Pasted image 20250702124426](https://github.com/user-attachments/assets/ac8cca6e-7e79-45be-b255-8f0bb183baae)











