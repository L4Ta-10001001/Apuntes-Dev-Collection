Hacemos el escaneo con nmap:
![[Pasted image 20240524183517.png]]
Esto es lo que corre por el puerto 80:
![[Pasted image 20240524183559.png]]
Utilizamos feroxbuster para enocntrar directorios web y vemos /admin y /login:
```bash
feroxbuster -u http://192.168.0.26/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
```
![[Pasted image 20240524183756.png]]
En el directorio de login, vemos lo siguiente:
![[Pasted image 20240524183829.png]]
Se nos indica que tenemos que autenticarnos como el usuario guest, y efectivamente estamos dentro:
![[Pasted image 20240524183908.png]]
Si consultamos el proceso o servicio de SSH, vemos que no nos muestra información y arriba en la url aparece una interrogación junto lo de admin, lo cual nos hace pensar que no somos el usuario admin y por tanto no podemos acceder a dicha información:
![[Pasted image 20240524184850.png]]
Y dentro de la web anterior, la de login donde pusimos guest:guest, tenemos una pista de como se tramita la información por detrás:
![[Pasted image 20240524185012.png]]
![[Pasted image 20240524185024.png]]
Ahora en la web de /admin, vamos a interceptar la petición con burpsuite:
![[Pasted image 20240524184850.png]]
![[Pasted image 20240524185328.png]]
Y modificaremos la petición por los siguiente valores, de tal forma que cambiamos el Content-Type en formato json para que la propiedad la acepte, esto nos debería permitir que el usuario guest obtenga el rol de admin:
```bash
application/json
admin_session=
{"__proto__":{"isAdmin":true}}
```
![[Pasted image 20240524185446.png]]
Si lanzamos la petición, vemos que funciona correctamente y ahora somos usuario admin:
![[Pasted image 20240524185619.png]]
Y para recibir la conexión, usaremos el payload usando una ssh:
![[Pasted image 20240524190002.png]]
![[Pasted image 20240524190011.png]]
Hay un archivo llamado ftp con unas credenciales:
```bash
carlton:gQzq2tG7sFxTm5XadrNfHR
```
![[Pasted image 20240524190237.png]]
Si consultamos las interfaces de red, vemos que estamos dentro de un contenedor de docker:
![[Pasted image 20240526103639.png]]
Nos compartimos chisel:
![[Pasted image 20240526103811.png]]
Desde la máquina atacante nos ponemos en escucha con chisel:
![[Pasted image 20240526103905.png]]
Y desde la víctima nos conectamos:
![[Pasted image 20240526103945.png]]
