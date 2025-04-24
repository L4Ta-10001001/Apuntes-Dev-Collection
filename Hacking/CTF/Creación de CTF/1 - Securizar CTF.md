Al exportar la máquina, marcamos esta opción para que no haya problemas de redes con la máquina:
![[Pasted image 20240516160838.png]]
### CIFRAR EL DISCO DURO
Para cifrar el disco de la máquina, lo hacemos dentro de su configuración:
### CREAR UN BANNER PERSONALIZADO
Para crear un banner cada vez que arrancamos la máquina,tenemos que editar el archivo /etc/issue y añadimos nuestro banner:
```bash
https://www.creativefabrica.com/es/tools/ascii-art-generator/
```
![[Pasted image 20240522160408.png]]
También podemos añadir más información:
![[Pasted image 20240523125922.png]]
### BORRADO DE LOGS
Para borrar logs y demás datos de la máquina, usamos los siguientes comandos:
```bash
ls -sf /dev/null /home/usuario/.bash_history
ls -sf /dev/null /root/.bash_history
```
**Eliminación de los logs de Apache y MySQL**
```bash
ln -sf /dev/null /var/log/apache2/access.log 
ln -sf /dev/null /var/log/apache2/error.log
ln -sf /dev/null /root/.mysql_history 
```
### CAMBIAR EL NOMBRE DE LA MÁQUINA
Para cambiar el nombre de la máquina, debemos de editar el archivo hostname:
```bash
nano /etc/hostname
```
![[Pasted image 20240523130043.png]]
Luego editamos el /etc/hosts:
![[Pasted image 20240523130206.png]]

### PROTEGER EL GRUB
Para proteger el grub, debemos seguir los siguientes pasos:
```bash
grub-mkpasswd-pbkdf2
```
![[Pasted image 20240523131247.png]]
Crear archivo, asignar permisos correcto y abrir:
```bash
touch /etc/grub.d/01_password && chmod a+x /etc/grub.d/01_password
nano /etc/grub.d/01_password
```
![[Pasted image 20240523131316.png]]
Y ahora editamos el siguiente archivo:
```bash
nano /etc/grub.d/01_password
```
Y pegamos el anterior:
```bash
#!/bin/sh
set -e
cat << EOF
set superusers="root"
password_pbkdf2 root <hash>
EOF
```
![[Pasted image 20240523131704.png]]
Ahora hay que abrir el siguiente archivo:
```bash
nano /etc/grub.d/10_linux
```
Y donde pone CLASS al final del todo ponemos --unrestricted:
![[Pasted image 20240523132524.png]]
Y ahora ejecutamos los siguientes comandos para aplicar cambios y reiniciar:
```bash
update-grub && reboot
```
