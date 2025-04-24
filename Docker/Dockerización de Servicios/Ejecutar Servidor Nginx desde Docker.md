Puedo tener un contenedor ejecutando un servidor nginx donde aloje un documento html y pueda acceder a él desde el navegador:

En primer lugar, debemos ejecutar el contenedor nginx y dentro del Dockerfile, debemos enviar el archivo html a la siguiente ruta que se nos muestra desde el repositorio oficial de nginx en dockerhub. Debemos primero escribir donde esté nuestro fichero html a la ruta que se indica en la captura de pantalla:

![[Pasted image 20221217110556.png]]
Primero creamos una carpeta donde enviaremos nuestro index.html:
![[Pasted image 20240304132522.png]]
Y ahora la especificamos dentro del Dockerfile:
```Dockerfile
FROM nginx

COPY carpeta_nginx /usr/share/nginx/html
```
![[Pasted image 20240304132617.png]]
Ahora monto la imagen con normalidad llamándola por ejemplo nginx_server:
![[Pasted image 20240304132646.png]]
Creamos la imagen:
![[Pasted image 20240304132701.png]]
Y ahora creamos el contenedor estableciendo el port forwarding. Con el -d indicamos que es un demonio, es decir, queremos que no se paralice el contenedor y se mantenga en ejecución.
```bash
docker run -d -p 8080:80 73
```
![[Pasted image 20240304132809.png]]
Y ahora si accedemos con el navegador al localhost:8080 tendremos acceso al fichero HTML dentro del servidor nginx dockerizado:
![[Pasted image 20240304132841.png]]
También aquí podemos lanzar el contenedor usando la red host para no tener que hacer el port forwarding:
```bash
docker run -d --network=host 73
```
Y ahora desde el propio localhost de ubuntu tendremos el contenedor funcionando:
![[Pasted image 20240304133002.png]]
