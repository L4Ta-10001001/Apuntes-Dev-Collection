Este contenedor será un wordpress con su respectiva base de datos MySQL donde el usuario registrado tendrá una contraseña débil la cual será vulnerable a un ataque de fuerza bruta con wpscan.

También, instalaremos un plugin para modificar temas de wordpress, de tal forma que el atacante pueda modificar el código de un archivo .php y así insertar codigo malicioso que le proporcione ejecución remota de comandos.

Una vez dentro del servidor, la escalada va a consistir en cambiar los permisos del binario /env, de tal forma que un atacante podrá utilizarlo para escalar privilegios.

En este caso, al tratarse de un wordpress, he considero más sencillo hacer toda la configuración creando primero un contenedor y hacerlo desde dentro, para finalmente convertir el contenedor a una imagen con docker commit.

---

Instalamos la base de datos:
```bash
apt install mariadb-server
```
Y habilitamos el servicio:
```bash
service mariadb start
service mariadb status
```
![[Pasted image 20240320151417.png]]
Ejecutamos el siguiente comando para configurar la base de datos:
```bash
mysql_secure_installation
```
![[Pasted image 20240320151631.png]]
Nos conectaremos a la base de datos con el siguiente comando y con la contraseña del usuario root que hayamos especificado anteriormente:
```bash
mysql -u root -pt9sH76gpQ82UFeZ3GXZS
```
![[Pasted image 20240320151743.png]]
Una vez dentro vamos a crear la base de datos de nuestro wordpress:
```bash
CREATE DATABASE wordpress
```
![[Pasted image 20240320151831.png]]
Vamos a crear un usuario para la base de datos de wordpress que es el que usaremos más adelante:
```bash
CREATE USER 'wordpressuser'@'%' IDENTIFIED BY 't9sH76gpQ82UFeZ3GXZS';
```
A continuación, deje saber a la base de datos que nuestro **wordpressuser** debería tener acceso completo a la base de datos que configuramos:
```bash
GRANT ALL ON wordpress.* TO 'wordpressuser'@'%';
FLUSH PRIVILEGES;
```
Y ya tenemos la base de datos configurada:
![[Pasted image 20240320170936.png]]
Ahora comenzamos con la instalación de WordPress, donde tenemos que ejecutar lo siguiente:
```bash
apt install php curl php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip apt php-mysql
```
Instalamos WordPress:
```bash
curl -O https://wordpress.org/latest.tar.gz
```
![[Pasted image 20240320152515.png]]
Lo extraemos con el comando:
```bash
tar xzvf latest.tar.gz
```
![[Pasted image 20240320152552.png]]
Ahora instalamos apache, que será lo que usemos para levantar el wordpress:
```bash
apt install apache2
```
Y copiamos el wordpress al directorio de apache:
```bash
cp -a wordpress/. /var/www/html/wordpress
```
![[Pasted image 20240320171257.png]]
Ahora haremos el dueño de todo el wordpress al usuario www-data:
```bash
sudo chown -R www-data:www-data /var/www/html/wordpress
```
A continuación, ejecutaremos dos comandos `find` para establecer los permisos correctos de los directorios y archivos de WordPress:
```bash
find /var/www/html/wordpress/ -type d -exec chmod 750 {} \;
find /var/www/html/wordpress/ -type f -exec chmod 640 {} \;
```
![[Pasted image 20240320152856.png]]
De esta forma, si accedemos al puerto 80 del contenedor tenemos la plantilla de apache por defecto:
![[Pasted image 20240320153243.png]]
Pero si vamos al directorio /wordpress, podremos comenzar con la configuración del CMS:
![[Pasted image 20240320153304.png]]
Y ahora configuraremos el acceso a la base de datos que hemos configurado anteriormente:
![[Pasted image 20240320173037.png]]
Y ya se ha instalado:
![[Pasted image 20240320173053.png]]
Y ponemos como nombre de usuario mario y de contraseña love, la cual será vulnerable a un ataque de fuerza bruta:
![[Pasted image 20240320173250.png]]
Probamos este acceso:
![[Pasted image 20240320173327.png]]
Y ha funcionado:
![[Pasted image 20240320173336.png]]
Y comprobamos que dicho usuario se ha registrado correctamente en la base de datos:
![[Pasted image 20240320173427.png]]
Ahora, para permitir que el atacante pueda obtener una reverse shell desde el panel de wordpress, lo que vamos a hacer será añadir un nuevo plugin que permita editar los temas:
![[Pasted image 20240320181131.png]]
Por tanto desde el menú de apariencia tendremos el menú de theme code editor, donde el atacante podrá insertar el código malicioso en php para obtener una reverse shell en el archivo index.php:
![[Pasted image 20240320181221.png]]
Este archivo index.php es el del tema; y el cual se encuentra dentro de la siguiente ruta:
```bash
http://172.17.0.2/wordpress/wp-content/themes/twentytwentytwo/index.php
```
![[Pasted image 20240320181251.png]]
Si con netcat permanecemos a la escucha, habremos recibido la conexión:
![[Pasted image 20240320181314.png]]
Ahora para la escalada de privilegios, vamos a configurar el binario /env con SUID 0:
```bash
chmod u+s /usr/bin/env
find / -perm -4000 2>/dev/null
```
Y lo localizamos:
![[Pasted image 20240320174731.png]]


