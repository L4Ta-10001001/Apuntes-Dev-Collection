Hacemos el escaneo de nmap:
![[Pasted image 20240105102735.png]]
Y esto corre por el puerto 80:
![[Pasted image 20240105102500.png]]
Hacemos fuzzing y nos encontramos con estos directorios:
![[Pasted image 20240105102855.png]]
Pero ambos directorios aparecen en blanco:
![[Pasted image 20240105102939.png]]
Pero dentro del directorio php-scripts vemos un archivo llamado file.php:
![[Pasted image 20240105103053.png]]
Que también aparece en blanco:
![[Pasted image 20240105103114.png]]
Y dentro del directorio /logs/ nos encontramos con el archivo vsftpd.log:
![[Pasted image 20240105103211.png]]
Nos lo bajamos:
![[Pasted image 20240105103230.png]]
Visualizamos el contenido del archivo y nos encontramos con un usuario existente en el sistema:
![[Pasted image 20240105103333.png]]
Vamos a seguir enumerando el directorio /file.php/ en busca de algún parámetro para explotar un LFI; y lo hacemos con wfuzz:
```bash
wfuzz -c --hc=404 --hl=0 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt  http://192.168.0.32/php-scripts/file.php?FUZZ=/etc/passwd
```
Y nos encuentra el número 6:
![[Pasted image 20240105103902.png]]
Y ahora con esta url habremos explotado un LFI y acceder al fichero /etc/passwd:
![[Pasted image 20240105103942.png]]
Y vemos un usuario existente llamado cromiphi:
![[Pasted image 20240105104040.png]]
Tenemos este usuario pero el puerto ssh aparece filtrado, por lo que no podemos entrar de esa forma. Por lo que primero intentamos acceder por ftp en el puerto 65535, por lo que usamos hydra:
```bash
hydra -t 64 -l admin_ftp -P /usr/share/wordlists/rockyou.txt ftp://192.168.0.32:65535
```
Y obtenemos las credenciales:
```bash
admin_ftp:cowboy
```
![[Pasted image 20240105104859.png]]
Y entramos vía ftp:
![[Pasted image 20240105104912.png]]
Y dentro de este servidor nos encontramos con estos 2 archivos:
![[Pasted image 20240105105006.png]]
Esto contiene la nota:
![[Pasted image 20240105105219.png]]
Y por otra parte, vamos a crackear este id_rsa:
![[Pasted image 20240105105026.png]]
Y obtenemos la contraseña ilovemyself:
![[Pasted image 20240105105108.png]]
Pero no tenemos abierto el puerto ssh, por lo que si investigamos, a partir del LFI explotado anteriormente podemos intentar obtener la IPv6 y por ahí intentar acceder al puerto ssh, por lo que para encontrar esta dirección, tenemos que apuntar a la siguiente ruta:
```bash
http://192.168.0.32/php-scripts/file.php?6=/proc/net/if_inet6
```
Y obtenemos la IPv6:
![[Pasted image 20240105105321.png]]
Lo vemos mejor presionando control + U viendo el código fuente:
![[Pasted image 20240105105416.png]]
No obstante, por aquí todavía no encontramos la IPv6; y para ello podemos utilizar ping6 de la siguiente forma:
```bash
ping6 -c2 -I eth0 ff02::1
```
Utilizamos los siguientes parámetros para ello:
- **`ping6`**: Este comando se utiliza para enviar paquetes de solicitud de eco ICMPv6 a una dirección específica.
    
- **`-c2`**: Indica que se enviarán 2 paquetes antes de detenerse.
    
- **`-I eth0`**: Especifica la interfaz de red desde la cual se enviarán los paquetes de ping. En este caso, se ha especificado la interfaz `eth0`.
    
- **`ff02::1`**: Esta es una dirección de multidifusión de enlace local. El rango `ff02::/16` está reservado para direcciones de multidifusión de enlace local, y el `::1` indica el grupo de todos los nodos en el enlace local.
Y nos devuelve esto, donde conseguimos muchas IPv6:
![[Pasted image 20240105112339.png]]
Vamos intentando enumerar con nmap hasta acertar:
![[Pasted image 20240105112700.png]]
Vamos probando en conectarnos por ssh con cada una de estas IPs hasta acertar:
```bash
ssh -6 -i id_rsa cromiphi@fe80::a00:27ff:fe4c:9dd4%eth0
```
Utilizando el archivo id_rsa con la contraseña ilovemyself del fichero id_rsa:
![[Pasted image 20240105112413.png]]
Si ejecutamos el comando sudo -l podemos ver que podemos ejecutar nmap como root:
![[Pasted image 20240105112817.png]]
En gtfobins tenemos las siguientes instrucciones:
![[Pasted image 20240105112928.png]]
Lo aplicamos y nos convertimos en root:
![[Pasted image 20240105112939.png]]
