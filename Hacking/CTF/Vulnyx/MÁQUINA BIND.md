Hacemos el escaneo de nmap:
![[Pasted image 20240307125951.png]]
Vemos que esto es lo que corre por el puerto 80:
![[Pasted image 20240307130027.png]]
Si hacemos fuzzing web sobre esta ruta vemos un directorio llamado /transmission:
![[Pasted image 20240307130124.png]]
Pero no tenemos permiso:
![[Pasted image 20240307130159.png]]
Y haciendo fuzzing sobre este directorio no tenemos nada:
![[Pasted image 20240307130251.png]]
Visualizamos ahora el puerto 8080 y vemos la misma plantilla de apache:
![[Pasted image 20240307130319.png]]
Hacemos fuzzing sobre este directorio y aquí sí nos encontramos con la ruta /stream:
![[Pasted image 20240307130359.png]]
Pero sobre este directorio nos ocurre lo mismo de antes, no tenemos acceso y por fuzzing no encontramos nada:
![[Pasted image 20240307130509.png]]
![[Pasted image 20240307130525.png]]
Pero estas dos rutas eran una pista del protocolo SCTP:
![[Pasted image 20240307130618.png]]
Por lo que hacemos una nueva enumeración de puertos con nmap pero en vez de usar el parametro -sS utilizaremos el parámetro -sY, el cual tiene algunas diferencias:
- `-sY`: Realiza un escaneo utilizando paquetes TCP ACK (Acknowledgment). En este tipo de escaneo, Nmap envía un paquete TCP ACK al puerto de destino y analiza la respuesta para determinar si el puerto está abierto, cerrado o filtrado.
- `-sS`: Realiza un escaneo utilizando paquetes TCP SYN (Synchronize). En este escaneo, Nmap envía un paquete SYN al puerto de destino y analiza la respuesta para determinar el estado del puerto.
- - `-sY`: Útil en situaciones donde otros métodos de escaneo son bloqueados por firewalls. Puede ser menos preciso en algunos casos debido a las variaciones en cómo los sistemas manejan los paquetes ACK.
- `-sS`: Método de escaneo estándar y ampliamente utilizado. Es eficiente y versátil, pero puede ser más fácilmente detectado por sistemas de detección de intrusiones.

Por tanto, hacemos un nuevo escaneo utilizando este parámetro y nos encontramos con el puerto 4444 abierto:
```bash
sudo nmap -p- -sS -sC -sV -sY --min-rate=5000 -n -vvv -Pn 192.168.0.17
```
![[Pasted image 20240307131430.png]]
Una vez hemos detectado este puerto, si lo hacemos con netcat no va a funcionar porque no está soportado, pero en cambio con ncat sí funciona:
![[Pasted image 20240307131622.png]]
Si ejecutamos el comando sudo -l vemos que podemos ejecutar el comando wtfutil como root:
![[Pasted image 20240307131714.png]]
Lo ejecutamos y vemos esto:
![[Pasted image 20240307131826.png]]
Vemos que al ejecutar este binario se crea un archivo llamado config.yml:
![[Pasted image 20240307131940.png]]
Y analizando este archivo podemos ver que por detrás se ejecuta el comando uptime:
![[Pasted image 20240307132014.png]]
Por lo que editamos este archivo config para que ejecute este otro comando, donde ejecutará el comando chmod con los siguientes argumentos:
![[Pasted image 20240307132142.png]]
Y ahora con el parámetro --config tenemos que pasarle este archivo de configuración que hemos editado:
![[Pasted image 20240307132334.png]]
```bash
sudo -u root /usr/bin/wtfutil --config=config.yml
```
![[Pasted image 20240307132426.png]]
