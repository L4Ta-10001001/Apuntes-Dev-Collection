Esta máquina vulnerable va a consistir en una versión vulnerable de Grafana (8.3.0) vulnerable a Local File Inclussion, la cual consiste en la capacidad de acceder a archivos internos de la máquina siempre y cuando el usuario tenga permisos para verlo. Por tanto, vamos a hacer que mediante un servidor FTP con el login como anonymous habilitado se tenga acceso a una base de datos de keepass, pero sin embargo no dispongamos de la contraseña.

La contraseña se encontrará ubicada en el directorio /tmp/pass.txt, que será accesible explotando la vulnerabilidad LFI del grafana vulnerable (la ruta /tmp/pass.txt la indicaremos como pista dentro del puerto 80 con apache).

Una vez explotada la vulnerabilidad, podremos abrir la base de datos de keepass para poder entrar como el usuario Freddy vía SSH. 

Una vez dentro del sistema, vamos a crear una escalada de privilegios que consista en el uso de un script de Python que siendo el usuario Freddy podamos ejecutar como root (editando el archivo sudoers). Una vez podamos editar dicho script vamos a insertar un código malicioso en Python que mediante el uso de la librería OS se cambien los permisos de /bin/bash para poder ejecutarla con privilegios de Root y así obtener dicha shell como root.

Utilizaremos el siguiente Dockerfile para construir toda la imagen de docker donde se hará lo siguiente:

1 - Primero debemos utilizar una imagen base de kali linux, debido a que tras varios intentos, grafana no funcionaba ni en ubuntu ni debian, pero sí en kali linux.

2 - Realizamos unas instalaciones básicas que utilizaremos en el resto del Dockerfile, así como la descarga y la instalación de grafana.

3 - Después, creamos el usuario Freddy con una contraseña robusta, la cual no será vulnerable a ningún ataque de fuerza bruta.

4 - Configuramos el servidor vsftpd para ser accesible como el usuario anonymous y poder visualizar la base de datos de KeePass.

5 - Damos la pista dentro del puerto 80 de que la contraseña de keepass se encuentra dentro de /tmp/pass.txt.

6 - Preparamos la escalada de privilegios con el script de python habiendo editado el archivo sudoers anteriormente.

---------
![[Pasted image 20240322112459.png]]
```bash
FROM kalilinux/kali-rolling

# Instala openssh-server, apache2, vsftpd y otras dependencias necesarias
RUN apt-get update && apt-get install -y openssh-server apache2 php vsftpd bash wget adduser libfontconfig1 musl 

RUN apt install -y python3 nano sudo

RUN wget https://dl.grafana.com/enterprise/release/grafana-enterprise_8.3.0_amd64.deb && \
    dpkg -i grafana-enterprise_8.3.0_amd64.deb

RUN useradd -m -s /bin/bash freddy && \
    echo 'freddy:t9sH76gpQ82UFeZ3GXZS' | chpasswd 

RUN mkdir -p /home/freddy/Desktop/

# Configura vsftpd
RUN echo "anonymous_enable=YES" >> /etc/vsftpd.conf
RUN echo "local_enable=YES" >> /etc/vsftpd.conf
RUN echo "write_enable=YES" >> /etc/vsftpd.conf
RUN echo "local_umask=022" >> /etc/vsftpd.conf
RUN echo "dirmessage_enable=YES" >> /etc/vsftpd.conf
RUN echo "xferlog_enable=YES" >> /etc/vsftpd.conf
RUN echo "connect_from_port_20=YES" >> /etc/vsftpd.conf
RUN echo "xferlog_std_format=YES" >> /etc/vsftpd.conf
RUN echo "listen=NO" >> /etc/vsftpd.conf
RUN echo "listen_ipv6=YES" >> /etc/vsftpd.conf
RUN echo "pam_service_name=vsftpd" >> /etc/vsftpd.conf
RUN echo "userlist_deny=NO" >> /etc/vsftpd.conf
RUN echo "tcp_wrappers=YES" >> /etc/vsftpd.conf

# Copia el archivo database.kdbx a la carpeta del usuario 'freddy'
COPY database.kdbx /srv/ftp/mantenimiento/

RUN chmod -R 777 /srv/ftp/mantenimiento/
# Copia el archivo de mantenimiento.html a la carpeta del servidor Apache
COPY maintenance.html /var/www/html/

# Contraseña del KeePass

RUN echo 't9sH76gpQ82UFeZ3GXZS' >> /tmp/pass.txt

# Crea el script de Python maintenance.py
RUN echo 'print("Server under beta testing")' > /opt/maintenance.py

RUN echo "freddy ALL=(ALL) NOPASSWD: /usr/bin/python3 /opt/maintenance.py" >> /etc/sudoers

RUN chown freddy:freddy /opt/maintenance.py

RUN chmod 777 /opt

EXPOSE 21 22 80

CMD service ssh start && service apache2 start && service grafana-server start && service vsftpd start && while true; do :; done
```
Una vez lo tengamos, ejecutamos el comando docker build y obtenemos la siguiente imagen:
![[Pasted image 20240322112528.png]]
Y ejecutamos el contenedor en segundo plano:
![[Pasted image 20240322112546.png]]
Tras comprobar que todo funciona correctamente, vamos a generar un archivo .tar y será lo que subamos a mega para después colgarlo en nuestra plataforma:
![[Pasted image 20240322112851.png]]
Y preparamos el script auto_deploy.sh:
```bash
#!/bin/bash

detener_y_eliminar_contenedor() {
    
    IMAGE_NAME="${TAR_FILE%.tar}"
    CONTAINER_NAME="${IMAGE_NAME}_container"

    if [ "$(docker ps -q -f name=$CONTAINER_NAME)" ]; then
        # Detiene el contenedor
        docker stop $CONTAINER_NAME
        echo "Contenedor detenido."
    else
        echo "No hay contenedor corriendo basado en la imagen."
    fi

    if [ "$(docker ps -a -q -f name=$CONTAINER_NAME)" ]; then
        docker rm $CONTAINER_NAME
        echo "Contenedor eliminado."
    else
        echo "No hay contenedor basado en la imagen."
    fi

    if [ "$(docker images -q $IMAGE_NAME)" ]; then
        docker rmi $IMAGE_NAME
        echo "Imagen eliminada."
    else
        echo "No hay imagen con ese nombre."
    fi
}

if [ $# -ne 1 ]; then
    echo "Uso: $0 <archivo_tar>"
    exit 1
fi

if ! command -v docker &> /dev/null; then
    echo "Docker no está instalado. Por favor, instale Docker para continuar."
    exit 1
fi

TAR_FILE="$1"

echo -e "\e[1mBienvenido al Script de Despliegue\e[0m"
echo ""

echo -e "\e[1;97m¿Qué acción deseas realizar?\e[0m"
echo ""
echo -e "\e[32m1. Desplegar el laboratorio\e[0m"
echo -e "\e[31m2. Eliminar el laboratorio\e[0m"
echo ""

read -p "Ingresa el número: " option
echo ""

case "$option" in 
    1)
        docker load -i "$TAR_FILE" > /dev/null
        docker container prune --force > /dev/null

        if [ $? -eq 0 ]; then
            echo -e "\e[93mEl archivo tar se ha cargado correctamente en Docker.\e[0m"

            IMAGE_NAME="${TAR_FILE%.tar}"
            CONTAINER_NAME="${IMAGE_NAME}_container"

            docker run -d --name $CONTAINER_NAME $IMAGE_NAME /bin/bash -c "while true; do echo 'Alive'; sleep 60; done" > /dev/null
            
            IP_ADDRESS=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $CONTAINER_NAME)
            echo -e "\e[96mLa dirección IP de la máquina objetivo es --> $IP_ADDRESS\e[0m"

        else
            echo -e "\e[91mHa ocurrido un error al cargar el archivo tar en Docker.\e[0m"
            exit 1
        fi
        ;;
    2)
        detener_y_eliminar_contenedor
        ;;
    *)
        echo -e "\e[91mOpción inválida. Por favor, selecciona una opción válida.\e[0m"
        exit 1
        ;;
esac
```
![[Pasted image 20240322112930.png]]
Lo comprimimos en formato .zip y lo subimos a mega:
![[Pasted image 20240322112951.png]]
Cargamos dicho enlace a nuestra web:
![[Pasted image 20240322113028.png]]
