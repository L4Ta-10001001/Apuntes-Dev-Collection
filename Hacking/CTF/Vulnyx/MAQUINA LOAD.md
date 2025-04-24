Hacemos el reconocimiento con nmap:
![[Pasted image 20240102104541.png]]
Y esto es lo que corre por el puerto 80:
![[Pasted image 20240102104609.png]]
Pero vemos que tenemos un robots.txt con el siguiente directorio:
![[Pasted image 20240102104631.png]]
Donde vemos que se trata de un CMS y además se nos muestra la versión:
![[Pasted image 20240102104658.png]]
Y haciendo fuzzing vemos un directorio de admin.php:
![[Pasted image 20240102104816.png]]
Accedemos y vemos que la contraseña es admin:admin:
![[Pasted image 20240102104856.png]]
Si miramos por internet exploits para la versión de ritecms 3.0, vemos que podemos obtener intrusión siguiendo los siguientes pasos:
```bash
https://www.exploit-db.com/exploits/50616
```
![[Pasted image 20240102104950.png]]
Preparamos nuestro php de pentestmonkey:
![[Pasted image 20240102105124.png]]
Y lo subimos:
![[Pasted image 20240102105304.png]]
Y ahora lo tenemos aquí:
![[Pasted image 20240102105316.png]]
Pero si hacemos clic sobre él no va a funcionar, porque en realidad la ruta será la siguiente, que es la que nos indican dentro de exploitdb:
![[Pasted image 20240102105358.png]]
Por lo que llamamos a la reverse shell siguiendo esta ruta y habremos recibido la conexión:
```bash
http://192.168.0.20/ritedev/media/php-reverse-shell.php
```
![[Pasted image 20240102105423.png]]
Una vez dentro, vemos que como travis podemos ejecutar el binario de crash:
![[Pasted image 20240102105518.png]]
En gtfobins vemos las instrucciones de cómo explotar este binario:
![[Pasted image 20240102105542.png]]
Por lo que lo ejecutamos con el panel de ayuda y luego lanzamos una sh:
```bash
sudo -u travis /usr/bin/crash -h
!sh
```
![[Pasted image 20240102105703.png]]
Y siendo el usuario travis, podemos ejecutar como root el binario de xauth:
![[Pasted image 20240102105919.png]]
Viendo las instrucciones vemos cómo utilizarlo:
![[Pasted image 20240102110051.png]]
Y tenemos esta parte donde podemos leer archivos:
![[Pasted image 20240105095700.png]]
Intentamos apuntar contra el fichero id_rsa de root, por lo que ejecutamos los siguientes comandos:
```bash
sudo -u 'root' /usr/bin/xauth
source /root/.ssh/id_rsa
```
Y leemos el id_rsa del usuario root:
![[Pasted image 20240105095842.png]]
Y nos lo copiamos en local:
![[Pasted image 20240105100034.png]]
Pero tenemos que readaptar las líneas que nos faltan tanto al principio como al final:
![[Pasted image 20240105100328.png]]
Y ahora, tras otorgar el permiso chmod 600 id_rsa ya podemos utilizarlo para acceder como root:
![[Pasted image 20240105100356.png]]
