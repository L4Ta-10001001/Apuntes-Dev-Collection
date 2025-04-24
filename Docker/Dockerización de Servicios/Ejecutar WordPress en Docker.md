Ejecutamos contenedores tanto de WordPress como de la base de datos:
```bash
docker run --name some-mysq -v /home/mario/Escritorio/volumen_mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my_secret_pw -d mysql
docker run --name wordpress -v /home/mario/Escritorio/volumen_wordpress:/var/www/html -d wordpress

```
![[Pasted image 20240317190407.png]]
![[Pasted image 20240317185716.png]]
Comprobamos que los volúmenes han funcionado correctamente:
![[Pasted image 20240317190750.png]]
Nos conectamos dentro de la base de datos y creamos la base de datos llamada wordpress:
```bash
create database wordpress;
```
![[Pasted image 20240317185801.png]]
Y desde el navegador accedemos al contenedor del wordpress y ponemos la información para conectarnos a la base de datos:
![[Pasted image 20240317191018.png]]
Y ya ha funcionado dicha conexión:
![[Pasted image 20240317185907.png]]
Comprobamos que se ha escrito información de wordpress en la base de datos:
![[Pasted image 20240317190159.png]]
Y configuramos los datos de acceso:
![[Pasted image 20240317185933.png]]
Comprobamos en la base de datos que se haya creado:
![[Pasted image 20240317190254.png]]
Y ya entramos dentro:
![[Pasted image 20240317190049.png]]
## COMPROBAR LA PERSISTENCIA DE INFORMACIÓN
Para comprobar que la información está correctamente almacenada en los volúmenes, vamos a borrar los contenedores del wordpress y mysql para volver a ejecutarlos y comprobar que se mantiene la información:
![[Pasted image 20240317191200.png]]
Desplegamos nuevamente los contenedores vinculando los volúmenes:
```bash
docker run --name wordpress -v /home/mario/Escritorio/volumen_wordpress:/var/www/html -d wordpress
docker run --name some-mysq -v /home/mario/Escritorio/volumen_mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql
```
Y ahora si accedemos a la web vemos que funciona correctamente y se mantiene la información:
![[Pasted image 20240317192151.png]]
# WORDPRESS + MYSQL EN RED HOST
Podemos hacer esto mismo pero configurándolo para que funcione dentro de la red host, pero será importante que nuestra máquina Ubuntu tenga una IP estática, para que así no haya problemas si borramos y ponemos nuevamente los contenedores, ya que estos tendrán configurada la IP que le pongamos y no puede cambiar:
![[Pasted image 20240317194033.png]]
A continuación, ya podemos desplegar los contenedores vinculándoles el volumen correspondiente:
```bash
docker run --name database -v /home/mario/Escritorio/volumen_mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw --network=host -d mysql
docker run --name wordpress -v /home/mario/Escritorio/volumen_wordpress:/var/www/html --network=host -d wordpress
```
![[Pasted image 20240317192523.png]]
Tenemos ambos contenedores desplegados:
![[Pasted image 20240317192534.png]]
De tal forma que si accedo a mi localhost, vemos que ya entramos en el wordpress:
![[Pasted image 20240317192609.png]]
En este punto entramos a la base de datos para crear la database llamada wordpress:
![[Pasted image 20240317193841.png]]
![[Pasted image 20240317193252.png]]
Y desde el navegador ponemos esta nueva base de datos y su correspondiente configuración:
![[Pasted image 20240317193436.png]]
Procedemos con los siguientes pasos:
![[Pasted image 20240317193510.png]]
Probamos en borrar y poner todo, y veremos que todo sigue igual:
![[Pasted image 20240317193654.png]]

