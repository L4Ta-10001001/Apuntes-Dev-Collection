## USO DE LA RED HOST
Para hacer que un contenedor utilice la red host con docker compose, hacemos lo siguiente:
```bash
version: '3.8'

services:
  
  fedora_container:
    image: fedora:latest
    command: ["bash", "-c", "sleep 60"]
    volumes:
      - /home/kali/Desktop/volumen_fedora:/home
    network_mode: host  # Establecer la red del host
```
![[Pasted image 20240318111002.png]]
Y si miramos la IP del contenedor, vemos que utiliza la misma que mi máquina anfitrión:
![[Pasted image 20240318111317.png]]
## USO DE LA RED BRIDGE
Sería exactamente igual pero no poniendo nada de las redes, y así se usa la red bridge ya que es la que usa docker por defecto.
## USO DE LA RED NONE
Lo hacemos igual pero especificando la red none dentro de la parte de network_mode:
```bash
version: '3.8'

services:
  
  fedora_container:
    image: ubuntu:latest
    command: ["bash", "-c", "sleep 60"]
    volumes:
      - /home/kali/Desktop/volumen_fedora:/home
    network_mode: none  # Establecer la red none
```
![[Pasted image 20240318111958.png]]
Lo ejecutamos y si vemos las interfaces, vemos que no tiene ninguna:
![[Pasted image 20240318112030.png]]
