# SERVIDOR
Instalamos openvpn:
```bash
apt install openvpn
```
Y ahora dentro del directorio `/etc/openvpn` tendremos los archivos de configuración de openvpn:
![[Pasted image 20240918125325.png]]
Y debemos de copiar el fichero de configuración de ejemplo para configurar el certificado:
```bash
sudo cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf .
```
![[Pasted image 20240918125517.png]]
Ahora vamos a copiarnos las claves:
```bash
sudo cp /usr/share/doc/openvpn/examples/sample-keys/{ca.crt,server.key,dh2048.pem,server.crt} .
```
![[Pasted image 20240918125847.png]]
A continuación editamos el archivo server.conf, donde pondremos nuestra IP:
![[Pasted image 20240918130022.png]]
Por aquí cargamos la ruta donde hayamos copiado las claves:
![[Pasted image 20240918130152.png]]
Y por aquí pondremos la IP que tendrá permiso para entrar a nuestra red:
![[Pasted image 20240918130253.png]]
Y por último tendremos que configurar y crear la clave que utilizarán los clientes para autenticarse:
![[Pasted image 20240918130428.png]]
Y generamos la clave:
```bash
sudo openvpn --genkey --secret ta.key
```
![[Pasted image 20240918130538.png]]
# CLIENTE
Ahora tenemos que configurar nuestra VPN que lo haremos con el siguiente comando:
```bash
sudo openvpn --config server.conf
```
![[Pasted image 20240918130713.png]]
Y veremos que hemos creado un túnel:
![[Pasted image 20240918130757.png]]
Ahora vamos a un cliente, que elegiremos por ejemplo un kali linux, donde nos ubicamos en `/etc/openvpn` y ahí creamos la carpeta kleys, donde copiaremos los siguientes archivos:
```bash
sudo cp /usr/share/doc/openvpn/examples/sample-keys/{ca.crt,client.crt,client.key} . 
```
![[Pasted image 20240918131202.png]]
Y ahora dentro del client.conf, tenemos que poner la IP pública de la máquina servidor, aunque en mi caso como estoy en una red interna pondré la IP privada del servidor:
![[Pasted image 20240918131402.png]]
Y también pondremos las rutas de las claves:
![[Pasted image 20240918131502.png]]
Y cargaremos también la ruta del ta.key:
![[Pasted image 20240918131616.png]]
Ahora vamos al servidor y compartimos la ta.key con el cliente:
```bash
sudo scp ta.key mario@192.168.222.174:/home/mario
```
![[Pasted image 20240918132026.png]]
Y lo traemos al directorio /keys del kali:
![[Pasted image 20240918132055.png]]
Y ahora podremos ejecutar el config.conf:
```bash
chmod 600 client.conf
```
![[Pasted image 20240918132313.png]]
Y ahora desde el Kali tendremos la interfaz tun0:
![[Pasted image 20240918132709.png]]
Y desde el linux mint también:
![[Pasted image 20240918132726.png]]
Y si hacemos un ping desde el kali a la linux mint por esa interfaz, habrá conexión:
![[Pasted image 20240918132752.png]]
E incluso podemos acceder a cualquier puerto de dicha maquina, por ejemplo si levantamos un servidor web, funcionará:
![[Pasted image 20240918132816.png]]
![[Pasted image 20240918132830.png]]
