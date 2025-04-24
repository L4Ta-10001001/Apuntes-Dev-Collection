## RED BRIDGE + OTRA INTERFAZ
Para conseguir que un contenedor de Docker pueda tener 2 interfaces de red o más, seguiremos los siguientes pasos, donde primero creamos dos redes con docker:
```bash
docker network create --subnet=10.10.10.0/24 pivoting1
docker network create --subnet=20.20.20.0/24 pivoting2
```
![[Pasted image 20240320094601.png]]
A continuación lanzamos un contenedor:
```bash
docker run -it --name pivoting redes
```
Y ahora conectamos estas dos redes que hemos creado anteriormente con el contenedor, pero desde otra pestaña del terminal, ya que tenemos que hacerlo desde nuestro Ubuntu:
```bash
docker network connect pivoting1 45
docker network connect pivoting2 45
```
![[Pasted image 20240320094730.png]]
Una vez hecho esto, si miramos dentro del contenedor, podremos ver las dos interfaces de red creadas:
![[Pasted image 20240320094759.png]]
## 2 INTERFACES DE RED INTERNAS
Si tenemos 2 redes creadas con docker y queremos asignarlas al mismo contenedor, lo que debemos hacer es lanzar el contenedor con una de las redes que ya tenemos creadas y luego conectarle la otra red:
```bash
docker run -it --network=pivoting1 redes
```
![[Pasted image 20240320095451.png]]
Y ahora desde otra pestaña, le conectamos la otra red de pivoting2:
```bash
docker network connect pivoting2 4a
```
![[Pasted image 20240320095621.png]]
