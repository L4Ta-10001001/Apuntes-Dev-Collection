Esta máquina tendrá un contenedor de Docker funcionando en su interior pero que no se encontrará expuesto. De tal forma, que dentro del contenedor va a estar corriendo un Grafana vulnerable a LFI. Por tanto, una vez hecha la intrusión por SSH con el usuario llamado Pinguinito, utilizaremos chisel para poder acceder al contenedor de Docker interno y así acceder al grafana que corre por el puerto 3000. 

Dentro del contenedor con el grafana vulnerable a LFI, tendremos un archivo llamado database_pass.txt que contendrá las credenciales de acceso a una base de datos de KeePass dentro del directorio /home de la máquina víctima. 

Dentro de esta base de datos tendremos las credenciales del usuario ballenasio y así poder acceder a este usuario que se encuentra dentro del grupo sudo y así poder convertirnos en root.

------------

El primer paso será descargar una imagen ISO de Ubuntu Server:
![[Pasted image 20240312114804.png]]
Y configuramos las credenciales y datos de acceso:
```bash
ballenasio:secureballenasio
```
![[Pasted image 20240312115131.png]]
![[Pasted image 20240312115443.png]]
El primer paso será crear un usuario nuevo que será el que se utilice para entrar mediante fuerza bruta por el protocolo SSH. El cual se llamará pinguinito, con la contraseña de love:
```bash
pinguinito:love
```
![[Pasted image 20240312115523.png]]
Ahora dentro de SSH, vamos a hacer que sólo se pueda acceder a esta máquina como el usuario pinguinito y no con ballenasio. Por lo que vamos a modificar el archivo de configuración de SSH:
```bash
sudo nano /etc/ssh/sshd_config
```
Y añadimos esta última línea:
![[Pasted image 20240312115911.png]]
Reiniciamos el servicio:
![[Pasted image 20240312120037.png]]
Y ahora comprobamos que sí podemos acceder como el usuario pinguinito pero no como el usuario ballenasio:
![[Pasted image 20240312120116.png]]
Ahora el siguiente paso será abrir el puerto 80, donde habrá una web muy sencilla que le de una pista al usuario de un usuario existente dentro de la máquina, el cual será el usuario pinguinito:
```bash
sudo apt install apache2
```
Y ahora con apache instalado, tendremos disponible la ruta /var/www/html:
![[Pasted image 20240312120234.png]]
Vamos a crear un nuevo archivo en .php que imprima un mensaje por pantalla diciendo: Hola Pinguinito:
```bash
<?php
echo "Hola Pinguinito. La ballena Docker con Grafana 8.3.0 esconde un database_pass.txt en /tmp";
?>
```
![[Pasted image 20240312142147.png]]
Y ahora reiniciamos el servicio y además con systemctl enable hacemos que se inicie cada vez que se arranque el sistema:
```bash
systemctl restart apache2
systemctl enable apache2
```
![[Pasted image 20240312120501.png]]
Instalamos php:
```bash
sudo apt install php
```
Podemos comprobar cómo nos encuentra ese penguin.php con gobuster:
![[Pasted image 20240312120801.png]]
![[Pasted image 20240312142221.png]]
Ahora vamos a instalar Docker, donde habrá un contenedor con una versión vulnerable de grafana al cual tendremos que hacer port forwarding con chisel:
```bash
sudo apt install docker.io
```
![[Pasted image 20240312121320.png]]
Una vez que docker esté instalado, vamos a crear una base de datos de keepass que estará dentro del sistema, y a su vez que la contraseña se encuentre dentro del directorio /etc/pass.txt:
```bash
supermegastrongpasswordpenguin
```
![[Pasted image 20240312121705.png]]
Y dentro de esta base de datos tendremos el acceso al usuario ballenasio:
![[Pasted image 20240312121823.png]]
Ahora vamos a tener esta base de datos dentro del directorio /home del servidor:
![[Pasted image 20240312122048.png]]
Y ahora a su vez, vamos a esconder un archivo .txt con la contraseña de la base de datos dentro de un contenedor con el grafana vulnerable a LFI:
![[Pasted image 20240312132344.png]]
Y ahora creamos el Dockerfile donde vamos a pasarle el database_pass.txt dentro del directorio /etc, para que el atacante pueda acceder a él mediante un LFI:
![[Pasted image 20240312141715.png]]
Y ahora creamos esta imagen:
```bash
sudo docker build --tag grafana_ctf .
sudo docker images
```
![[Pasted image 20240312122620.png]]
Y dentro del contenedor creamos el archivo database_pass.txt en el directorio /tmp:
![[Pasted image 20240312141507.png]]
Y convertimos este contenedor a una imagen:
```bash
docker commit 49 grafana_vulnerable
```
![[Pasted image 20240312141649.png]]
Una vez hecho esto, debemos conseguir que el sistema inicie este contenedor cada vez que se arranque, por lo que lo añadiremos a crontab de la siguiente forma:
```bash
crontab -e
```
![[Pasted image 20240312141746.png]]
Reiniciamos el sistema para comprobar si a cada reinicio se lanza un contenedor de este grafana:
![[Pasted image 20240312123240.png]]
