Este contenedor sólamente va a tener el protocolo FTP habilitado, pero podremos entrar como el usuario bob debido a que su contraseña será password1 (vulnerable a fuerza bruta) y en su interior habrá un script llamado autorun.sh, el cual se estará ejecutando con crontab de forma automatizada. Por tanto, como atacantes, lo que debemos de hacer es sustituir el código de ese archivo e insertar una reverse shell para adquirir intrusión al servidor:

---------------

```bash
FROM debian

RUN apt-get update && apt-get install -y vsftpd

# Agregar el usuario "bob" con la contraseña proporcionada
RUN useradd -m -s /bin/bash bob \
    && echo "bob:password1" | chpasswd

WORKDIR /home/bob

# Configura vsftpd
RUN echo "local_enable=YES" >> /etc/vsftpd.conf
RUN echo "write_enable=YES" >> /etc/vsftpd.conf
RUN echo "local_umask=022" >> /etc/vsftpd.conf
RUN echo "dirmessage_enable=YES" >> /etc/vsftpd.conf
RUN echo "xferlog_enable=YES" >> /etc/vsftpd.conf
RUN echo "connect_from_port_20=YES" >> /etc/vsftpd.conf
RUN echo "xferlog_std_format=YES" >> /etc/vsftpd.conf
RUN echo "userlist_deny=NO" >> /etc/vsftpd.conf
RUN echo "tcp_wrappers=YES" >> /etc/vsftpd.conf

# Copia el script de limpieza al contenedor
RUN echo 'echo autoclean' >> /home/bob/clean.sh
# Permisos para el script de limpieza
RUN chmod 777 clean.sh

# Exponer puerto 21 para vsftpd
EXPOSE 21

# Inicia vsftpd y ejecuta el script de limpieza como el usuario bob
CMD service vsftpd start && while true; do su - bob -c "./clean.sh"; sleep 3600; done
```
Lo primero será crear un usuario llamado administración, que será el usuario que utilizaremos para hacer la intrusión inicial. Este usuario tendrá una contraseña robusta, ya que la intrusión no será por fuerza bruta:
```bash
administracion:t9sH76gpQ82UFeZ3GXZS
```
![[Pasted image 20240320112310.png]]
Instalamos vsftpd y lo habilitamos para que se pueda entrar como el usuario anonymous:
```bash
apt install vsftpd
```
Habilitamos el login como el usuario anonymous:
![[Pasted image 20240320112615.png]]
Y también configuramos que el usuario anonymous tenga permisos de escritura:
![[Pasted image 20240320114533.png]]
Y permitimos que pueda subir archivos:
![[Pasted image 20240320114923.png]]
Y permitimos que pueda crear carpetas:
![[Pasted image 20240320114946.png]]
Ahora, dentro de la ruta /srv/ftp debemos de crear un nuevo directorio que será donde incluyamos el script:
```bash
mkdir mantenimiento
```
Y a este directorio debemos cambiarle el propietario de grupo al usuario ftp:
```bash
chown :ftp mantenimiento/
chmod g+w mantenimiento
```
![[Pasted image 20240320120526.png]]
Reiniciamos el servicio de vsftpd:
![[Pasted image 20240320112754.png]]
Desde otra máquina, comprobamos que el protocolo FTP esté habilitado en este contenedor:
![[Pasted image 20240320112848.png]]
A continuación, vamos a insertar un archivo en esta misma ubicación que limpie el contenido del archivo /tmp y en este mismo directorio genere un archivo avisando de que la limpieza se hizo correctamente, especificando la hora exacta:
```bash
#!/bin/bash

LOG_FILE="/tmp/reporte.log"

get_current_time() {
    date +"%Y-%m-%d %T"
}

echo "$(get_current_time): Eliminando archivos en /tmp..." | tee -a "$LOG_FILE"
rm -rf /tmp/* >> "$LOG_FILE" 2>&1

if [ $? -eq 0 ]; then
    echo "$(get_current_time): Los archivos en /tmp han sido eliminados correctamente." | tee -a "$LOG_FILE"
else
    echo "$(get_current_time): Hubo un error al intentar eliminar los archivos en /tmp." 1>&2 | tee -a "$LOG_FILE"
    exit 1
fi

exit 0
```
![[Pasted image 20240320121021.png]]
Y vemos que al momento de ejecutarlo, se genera dicho archivo con la hora:
![[Pasted image 20240320121101.png]]
Ahora debemos de hacer como propietario de grupo de este archivo al usuario FTP, para que el usuario anonymous lo pueda modificar, así como establecer los permisos correctos para los grupos:
![[Pasted image 20240320121228.png]]


