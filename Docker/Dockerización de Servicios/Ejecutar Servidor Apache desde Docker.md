El primer paso será seguir las instrucciones de la imagen oficial de apache en dockerhub:
![[Pasted image 20240304131329.png]]
Por lo que creamos el siguiente Dockerfile utilizando dicha imagen:
```Dockerfile
FROM httpd:latest

COPY index.html /usr/local/apache2/htdocs/
```
![[Pasted image 20240304131351.png]]
Preparamos el archivo HTML que queramos dockerizar:
![[Pasted image 20240304131526.png]]
Y una vez hecho esto, lanzamos una imagen con docker buildx a partir de este Dockerfile
```bash
docker buildx build --tag server_apache .
```
![[Pasted image 20240304131452.png]]
Y ahora lanzaremos un contenedor de esta imagen diciendo que el puerto 80 del contenedor será el puerto 9090 de mi máquina Ubuntu:
```bash
docker run -d -p 9090:80 ca
```
![[Pasted image 20240304131621.png]]
Consultamos el puerto 9090 de nuestro Ubuntu y vemos que accedemos correctamente al puerto 80 del contenedor de Apache:
![[Pasted image 20240304131648.png]]
### Red Host en el Contenedor de Apache
También podemos cargarle la red host a este contenedor de Apache y así no tener que hacer el port forwarding, por lo que ejecutamos lo siguiente al momento de lanzar el contenedor:
```bash
docker run -d --network=host ca
```
![[Pasted image 20240304131851.png]]
Y si accedemos al localhost, veremos el puerto 80 del contenedor, ya que el contenedor utiliza nuestra misma dirección IP:
![[Pasted image 20240304131922.png]]
