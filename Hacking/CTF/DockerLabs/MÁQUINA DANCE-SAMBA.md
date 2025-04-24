Hacemos el escaneo con nmap:
![[Pasted image 20240825212002.png]]
Vamos a ejecutar enum4linux para enumerar el puerto 445 samba:
```bash
enum4linux 172.17.0.2
```
![[Pasted image 20240825212055.png]]
Nos encontramos con el usuario macarena, donde probamos con smbclient si podemos listar recursos compartidos, pero no podemos, por lo que probamos en entrar por FTP como usuario anonymous:
```bash
ftp anonymous@172.17.0.2
```
![[Pasted image 20240825212125.png]]
Y vemos una nota.txt, donde nos encontramos con el nombre donald, que quizá es la contraseña de macarena:
```bash
macarena:donald
```
Por lo que lo probamos y funciona:
```bash
smbclient -L //172.17.0.2/ -U macarena
```
![[Pasted image 20240825212146.png]]
Vamos a entrar al recurso compartido de macarena usando smbmap:
```bash
smbmap -H 172.17.0.2 -u macarena -p donald -r macarena
```
![[Pasted image 20240825212432.png]]
Veremos que `samba` esta compartiendo de forma local la carpeta del usuario `macarena` en el servidor victima, pero no podremos salirnos de esta carpeta. Lo que si podemos hacer es crear y cargar carpetas y archivos, por o que haremos lo siguiente.

Crearemos la carpeta `.ssh/` y dentro de ella cargaremos nuestra `id_rsa.pub` de nuestro host a parte de crear un archivo `authorized_keys` que contenga nuestro `id_rsa.pub` para podernos conectar desde el ssh proporcionando nuestra `id_rsa` de nuestro host.
![[Pasted image 20240825212609.png]]
Ahora nos creamos la clave pública de nuestra máquina atacante y la copiaremos al directorio .ssh:
```bash
ssh-keygen -t rsa -b 4096
cp /root/.ssh/id_rsa.pub .
```
![[Pasted image 20240825212828.png]]
![[Pasted image 20240825212841.png]]
Y ahora vamos a renombrar este archivo a authorized_keys:
```bash
cp id_rsa.pub authorized_keys
```
Y ahora este archivo lo subiremos al servidor samba, por lo que nos conectamos por smbclient y con el comando put lo subimos:
```bash
smbclient //172.17.0.2/macarena -U macarena
```
En este punto, tenemos que crear el directorio .ssh/ dentro del servidor samba y ahí mismo subir el archivo authorized_keys que será la clave pública de la máquina atacante, y así poder entrar al servidor:
![[Pasted image 20240826121915.png]]
Y también tendremos que tener el id_rsa.pub:
![[Pasted image 20240826122213.png]]
Posteriormente, usando le clave privada de nuestra máquina atacante generada anteriormente podremos usarla para conectarnos de forma automática con el servidor:
![[Pasted image 20240826122227.png]]
Dentro del directorio /home vemos un hash, el cual si se lo pasamos a chatgpt nos lo proporciona en texto plano:
![[Pasted image 20240826122642.png]]
Obteniendo así la siguiente contraseña:
```bash
macarena:supersecurepassword
```
Tras ejecutar el comando sudo -l podemos ver que podemos ejecutar el comando file como root:
![[Pasted image 20240826122809.png]]
Dentro de /opt vemos un archivo donde no tenemos permisos de lectura:
![[Pasted image 20240826122929.png]]
Con el comando find vamos a intentar visualizarlo:
![[Pasted image 20240826123012.png]]
Nos autenticamos con la contraseña de root y ya habremos escalado:
```bash
root:rooteable2
```

