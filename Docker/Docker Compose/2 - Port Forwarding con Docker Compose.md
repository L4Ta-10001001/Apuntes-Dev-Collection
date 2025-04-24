Podemos establecer el reenvío de puertos de la siguiente forma, donde decimos que el puerto 9000 sea el puerto 8000 del contenedor en la parte de ports:
```bash
version: '3.8'

services: # Aquí incluimos los contenedores.
  
  jenkins_contenedor:
    image: jenkins/jenkins
    ports:
     - "9000:8080"

  fedora_container:
    image: fedora:latest
    command: ["bash", "-c", "sleep 60"]
```
![[Pasted image 20240318110039.png]]
Ejecutamos el comando docker-compose up -d y tenemos los dos contenedores funcionando y el de jenkins con el port forwarding:
![[Pasted image 20240318110117.png]]
Y ahora vemos que el port forwarding del jenkins funciona bien y lo tenemos corriendo por el puerto 9000:
![[Pasted image 20240318110142.png]]
