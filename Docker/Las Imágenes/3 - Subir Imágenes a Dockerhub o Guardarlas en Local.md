Debemos ejecutar estos 3 comandos:
```bash
docker commit 33268dd332ef laboratorio2
docker login
docker tag laboratorio3 maalfer/laboratorio3:latest
docker push maalfer/laboratorio3:latest
```
Y lo tenemos:
![[Pasted image 20240320175435.png]]
Si queremos exportar una imagen en local, utilizaremos docker save y docker load. Por ejemplo, vamos a exportar la imagen upload:
```bash
docker save -o upload.tar upload:latest
```
![[Pasted image 20240327133655.png]]
Ahora si queremos importar esta imagen, utilizamos el comando docker load -i:
```bash
docker load -i upload.tar
```
![[Pasted image 20240327133739.png]]
