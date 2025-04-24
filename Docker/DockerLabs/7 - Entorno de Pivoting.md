Creamos primero las 4 redes que nos harán falta:
```bash
docker network create --subnet=10.10.10.0/24 pivoting1
docker network create --subnet=20.20.20.0/24 pivoting2
docker network create --subnet=30.30.30.0/24 pivoting3
docker network create --subnet=40.40.40.0/24 pivoting4
```
![[Pasted image 20240329093742.png]]
Ahora vamos a importar las distintas máquinas en forma de imágenes con el comando docker load -i:
![[Pasted image 20240410121105.png]]
Y verificamos que tenemos todas las máquinas importadas correctamente:
![[Pasted image 20240410121127.png]]
