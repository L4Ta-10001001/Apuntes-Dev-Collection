Para configurar las redes de Linux, tendremos que modificar el fichero de configuración de netplan, que lo encontramos en la ruta /etc/netplan:
![[Pasted image 20230119203125.png]]
Este fichero con extensión .yaml será el que tengamos que modificar:
![[Pasted image 20230119203207.png]]
Y podremos añadir nosotros la configuración que queramos:
```bash
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      addresses: [192.168.0.25/24]  # Ponemos la IP que queramos
      gateway4: 192.168.0.1  # La puerta de enlace
      nameservers:
        addresses: [8.8.8.8]  # Servidores DNS
      dhcp4: false  # Desactivamos el protocolo DHCP.
  version: 2
```
![[Pasted image 20230119203731.png]]
Ahora sólo tenemos que aplicar los cambios con el comando netplan apply:
![[Pasted image 20230119203853.png]]
Y si hacemos un ifconfig ya vemos la nueva configuración:
![[Pasted image 20230119203916.png]]
