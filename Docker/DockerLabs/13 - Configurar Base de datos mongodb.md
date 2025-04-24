Para que la información persista, debemos de crear la base de datos desde fuera y transferir el .json con la información a la máquina víctima, donde ejecutaremos el siguiente comando:
```bash
mongoimport --db accesos --collection usuarios --file accesos.usuarios.json --jsonArray
```
![[Pasted image 20240516135933.png]]
Y ya lo tendremos importado:
![[Pasted image 20240516135951.png]]
Si lo queremos configurar en docker, debemos de establecer así el dockerfile para que la información de la base de datos persista:
```Dockerfile
FROM mongo:latest

COPY accesos.usuarios.json /opt

CMD service ssh start && \
    service mariadb start && \
    service apache2 start && \
    mongod --bind_ip_all --quiet & \   
    sleep 10 && \    
    mongoimport --db accesos --collection usuarios --file /opt/accesos.usuarios.json --jsonArray && \
    rm /opt/accesos.usuarios.json && \
    tail -f /dev/null
```
