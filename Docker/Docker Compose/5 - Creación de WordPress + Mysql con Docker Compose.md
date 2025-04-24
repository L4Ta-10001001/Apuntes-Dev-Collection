Vamos a levantar tanto un contenedor de wordpress como de MySQL con el siguiente archivo docker-compose.yml:
```bash
version: '3.8'

services:
  
  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: examplepassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    network_mode: host  # Utilizar la red del host

  wordpress:
    image: wordpress:latest
    environment:
      WORDPRESS_DB_HOST: 127.0.0.1
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    network_mode: host  # Utilizar la red del host

```
Ahora al desplegar esto, tenemos la base de datos corriendo en nuestro localhost por el puerto 3306 y el wordpress por el puerto 80, donde si accedemos a este puerto, ya podremos acceder al wordpress:
![[Pasted image 20240318113321.png]]
Así como a la base de datos con el usuario wordpress y contraseña wordpress:
```bash
mysql -u wordpress -h 192.168.0.21 -pwordpress
```
![[Pasted image 20240318113350.png]]
Y vemos como la información se ha escrito correctamente dentro de la base de datos de wordpress:
![[Pasted image 20240318113425.png]]
# WORDPRESS + MYSQL CON VOLÚMENES
Podemos hacer lo mismo pero creando volúmenes tanto para el wordpress como para la base de datos. Por lo que primero vamos a crear ambos directorios:
![[Pasted image 20240318113744.png]]
Y creamos el docker-compose.yml:
```bash
version: '3.8'

services:

  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: examplepassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    network_mode: host  # Utilizar la red del host
    volumes:
      - /home/mario/Escritorio/mysql_volumen:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    environment:
      WORDPRESS_DB_HOST: 127.0.0.1
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    network_mode: host  # Utilizar la red del host
    volumes:
      - /home/mario/Escritorio/wordpress_volumen:/var/www/html/wp-content
```
Desplegamos el docker-compose:
![[Pasted image 20240318113815.png]]
Y ya vemos como los volúmenes contienen la información de los contenedores:
![[Pasted image 20240318113840.png]]
Accedemos al puerto 80 y procedemos con la configuración:
![[Pasted image 20240318113917.png]]
Entramos como Mario:
![[Pasted image 20240318114008.png]]
Eliminamos los contenedores:
![[Pasted image 20240318114134.png]]
Y volvemos a desplegarlos:
![[Pasted image 20240318114153.png]]
Y vemos como la información persiste:
![[Pasted image 20240318114214.png]]
Y la base de datos también contiene la misma información:
![[Pasted image 20240318114249.png]]
