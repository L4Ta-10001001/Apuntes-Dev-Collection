Hacemos el escaneo con nmap:
![[Pasted image 20240130154025.png]]
Y esto corre por el puerto 80:
![[Pasted image 20240130154041.png]]
Y si hacemos fuzzing web nos encontramos con el siguiente directorio de /my_weblog:
![[Pasted image 20240130154136.png]]
Y esto es lo que corre:
![[Pasted image 20240130154158.png]]
Para conocer el CMS al que nos estamos enfrentando, utilizamos whatweb y vemos que se trata de un nibbleblog:
![[Pasted image 20240130154302.png]]
Si queremos conocer la versión, tenemos la siguiente ruta donde se muestra la versión del CMS nibbleblog:
```bash
curl -X GET 'http://192.168.0.60/my_weblog/admin/boot/rules/98-constants.bit'
```
![[Pasted image 20240130154808.png]]
Y tenemos este repositorio donde se nos dice que en versiones 4.0.3 (más recientes) podemos subir archivos, por lo que suponemos que para la versión 3.7.1 también sea vulnerable:
![[Pasted image 20240130154917.png]]
Pero vemos que nos piden estar autenticados:
![[Pasted image 20240130155009.png]]
No obstante, dentro del exploit nos dicen que hay una ruta llamada admin.php:
![[Pasted image 20240130155040.png]]
```bash
http://192.168.0.60/my_weblog/admin.php
```
La probamos y encontramos el panel de login:
![[Pasted image 20240130155100.png]]
Interceptamos la petición y vemos la ruta hasta llegar al panel de login:
![[Pasted image 20240130155417.png]]
Y utilizamos hydra para hacer este ataque utilizando la información obtenida con burp suite:
```bash
hydra -t 64 -l admin -P /usr/share/wordlists/rockyou.txt 192.168.0.60 http-post-form "/my_weblog/admin.php:username=admin&password=^PASS^:Incorrect username or password."
```
![[Pasted image 20240130155707.png]]
```bash
admin:kisses
```
Y estamos dentro:
![[Pasted image 20240130160102.png]]
Y vemos que en el exploit nos indican la ruta para subir el archivo:
![[Pasted image 20240130160152.png]]
Y por aquí vemos el lugar para subir un archivo malicioso:
![[Pasted image 20240130160411.png]]
Subimos una reverse shell en php y accedemos a la ruta que hemos visto del script pero antes tenemos que renombrar la reverse shell a image.php, ya que es el nombre que espera la web:
![[Pasted image 20240130160847.png]]
![[Pasted image 20240130160901.png]]
Accedemos a la ruta donde se subió el image.php malicioso:
```bash
192.168.0.60/my_weblog/content/private/plugins/my_image/image.php
```
Y recibimos la conexión:
![[Pasted image 20240130160949.png]]
Si ejecutamos el comando sudo -l vemos que podemos ejecutar git como root:
![[Pasted image 20240130161104.png]]
Y debemos de ejecutar el siguiente comando para ejecutar el panel de ayuda de git y colar un comando:
![[Pasted image 20240130161304.png]]
```bash
sudo -u admin /usr/bin/git branch --help config
```
![[Pasted image 20240130161240.png]]
Y ya somos el usuario admin, donde vemos que si ejecutamos el comando sudo -l y podemos ejecutar este otro binario como root:
![[Pasted image 20240130161627.png]]
Lo ejecutamos y si presionamos el atajo control F podemos desplegar un menú:
![[Pasted image 20240130161523.png]]
Entramos dentro de user menu y luego en invoke shell:
![[Pasted image 20240130161550.png]]
Y ya somos root:
![[Pasted image 20240130161609.png]]
