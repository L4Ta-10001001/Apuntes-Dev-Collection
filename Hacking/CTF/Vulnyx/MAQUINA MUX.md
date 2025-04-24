Hacemos el escaneo con nmap:
![[Pasted image 20240112114124.png]]
Y esto es lo que tenemos dentro del puerto 80:
![[Pasted image 20240112114203.png]]
Nos descargamos esta imagen y con exiftool vemos si hay algo dentro aplicando esteganografía, donde encontramos unas credenciales:
```bash
lisa:My_$3cUr3_RSH_p@zz
```
![[Pasted image 20240112114344.png]]
Y también si visualizamos la imagen con cat o con un strings vemos otras credenciales:
```bash
lisa:Gi0c0nd@
```
![[Pasted image 20240112114447.png]]
Una vez revisado esto, podemos ver que hay dentro del puerto 514, pero el acceso está restringido:
![[Pasted image 20240112114541.png]]
Investigamos por internet y tenemos este artículo de hacktricks:
```bash
https://book.hacktricks.xyz/network-services-pentesting/pentesting-rsh
```
![[Pasted image 20240112114623.png]]
Instalamos el cliente de rsh:
```bash
apt install rsh-client
```
Una vez instalado el cliente, probamos a entrar de la siguiente forma con la contraseña Gi0c0nd@:
```bash
rsh 192.168.0.40 -l lisa
```
![[Pasted image 20240112114919.png]]
Si ejecutamos el comando sudo -l, podemos ver cómo podemos ejecutar como root tmux:
![[Pasted image 20240112115003.png]]
Vemos que con ejecutarlo como sudo simplemente ya podemos escalar privilegios:
![[Pasted image 20240112115028.png]]
Lo ejecutamos y nos convertimos en root:
![[Pasted image 20240112115051.png]]
