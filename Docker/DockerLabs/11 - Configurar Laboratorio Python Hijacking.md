Esta vulnerabilidad funciona al importar una librería, pero si en el mismo directorio de trabajo tenemos otro script con el nombre de la librería.py, se llamará primero a ese código y podremos ejecutar código malicioso:
```python
# Usa la imagen base de Ubuntu
FROM ubuntu:latest

# Actualiza el índice de paquetes e instala Python y pip
RUN apt-get update && \
    apt-get install -y python3 nano sudo apache2 python3-pip openssh-server

RUN useradd -m -s /bin/bash carlos \
    && echo "carlos:JIFGHDS87GYDFIGD" | chpasswd

COPY index.php /var/www/html

# Crea el script Python dentro del contenedor que usa la librería shutil
RUN echo "\
import shutil\n\
\n\
def copiar_archivo(origen, destino):\n\
    shutil.copy(origen, destino)\n\
    print(f'Archivo copiado de {origen} a {destino}')\n\
\n\
if __name__ == '__main__':\n\
    origen = '/opt/script.py'\n\
    destino = '/tmp/script_backup.py'\n\
    copiar_archivo(origen, destino)\n\
" > /opt/script.py

RUN echo "carlos ALL=(ALL) NOPASSWD: /usr/bin/python3 /opt/script.py" >> /etc/sudoers

# Cambia los permisos del directorio /opt para que el usuario 'developer' tenga permisos de escritura
RUN chown -R carlos /opt && chmod u+w /opt

RUN chmod u+x /opt/script.py && chmod u-w /opt/script.py

CMD service ssh start && service apache2 start && tail -f /dev/null
```