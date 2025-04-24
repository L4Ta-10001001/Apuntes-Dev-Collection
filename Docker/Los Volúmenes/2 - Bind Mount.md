Primero creamos un directorio en nuestro host:
![[Pasted image 20240301160723.png]]
Y ahora al desplegar el contenedor podemos cargarle esta carpeta:
```bash
docker run -it -v /home/mario/Escritorio/data_host:/home 3d
```
Y le creamos un archivo al directorio /home:
![[Pasted image 20240301160809.png]]
Y este archivo se habrá creado en la carpeta también:
![[Pasted image 20240301160825.png]]
