### CREAR UN DISCO Y PARTICIÓN
Podemos cifrar el disco de nuestro sistema Linux utilizando LUKS, de esta forma si un atacante accede a nuestro disco no podrá acceder a la información a no ser que introduzca la contraseña. Por lo que el primer paso será ejecutar el comando `lsblk` para ver los discos duros y particiones:
![[Pasted image 20240828143242.png]]
Y ahora vamos a añadir un nuevo disco duro en virtualbox:
![[Pasted image 20240828143435.png]]
Ahora arrancamos la máquina nuevamente y tras ejecutar el mismo comando de antes ya tendremos el nuevo disco duro insertado:
![[Pasted image 20240828143532.png]]
Una vez hecho esto, vamos a ejecutar fdisk sobre este disco para crearle una partición:
```bash
fdisk /dev/sdb
```
![[Pasted image 20240828143710.png]]
Una vez creada la partición, ya podremos hacer uso del disco:
![[Pasted image 20240828143822.png]]
Ahora si ejecutamos gparted, podemos visualizar nuestra nueva partición:
![[Pasted image 20240828143943.png]]
Ahora ponemos w para escribir en el disco estos cambios y podemos visualizar nuevamente la pariticón al ejecutar el comando `lsblk`:
![[Pasted image 20240828144031.png]]
Ahora, para poder acceder a esta partición tenemos que montarla en mnt:
```bash
mount /dev/sdb1 /mnt
df -h | grep /mnt
```
![[Pasted image 20240828145506.png]]
Nos creamos un archivo secreto en la partición:
![[Pasted image 20240828145636.png]]
### CIFRAR LA PARTICIÓN
Para cifrar la partición tenemos que asegurarnos que no esté montada:
```bash
umount /dev/sdb1 /mnt
```
![[Pasted image 20240828145957.png]]
Y ahora con la herramienta cryptsetup lo ciframos estableciendo la contraseña 123123 (por ejemplo):
![[Pasted image 20240828150025.png]]
Ahora para ver el disco cifrado por LUKS podemos ejecutar el comando `lsblk -f`y lo veremos cifrado:
![[Pasted image 20240828150132.png]]
Ahora tenemos que desbloquear esta partición y darle un formato, montarla donde corresponda y así poder usarla, por lo que ejecutamos el siguiente comando `blkid` y copiamos lo que está marcado:
![[Pasted image 20240828150702.png]]
Y nos abrimos el archivo /etc/crypttab y añadimos la siguiente información, estableciendo el nombre de neustro disco y el identificador:
```bash
sdb1_encriptada: UUID="fb741601-2e24-49e5-8919-b65f75d2dba6" none luks
```
![[Pasted image 20240828150847.png]]
Ahora debemos desbloquear esta partición de la siguiente forma, donde ahora ya tendremos la partición disponible:
```bash
cryptsetup luksOpen /dev/sdb1 sdb1_encriptada
```
![[Pasted image 20240828151019.png]]
Y ahora aplicamos un formateo a esta partición desbloqueada:
```bash
mkfs.ext4 /dev/mapper/sdb1_encriptada
```
![[Pasted image 20240828151210.png]]
Una vez hecho esto, ya podremos montar la partición por ejemplo en el directorio /mnt y creando una carpeta llamada partición_encriptada:
```bash
mount -t ext4 /dev/mapper/sdb1_encriptada particion_encriptada
```
![[Pasted image 20240828151453.png]]
Llegados a este punto, yo puedo suspender el acceso a este disco con el siguiente comando:
```bash
cryptsetup luksSuspend sdb1_encriptada
```
Y ahora mismo no podremos escribir en el disco:
![[Pasted image 20240828151716.png]]
Ahora para desbloquear el disco, tenemos que ejecutar el siguiente comando y además proporcionar la contraseña:
```bash
cryptsetup luksResume sdb1_encriptada
```
![[Pasted image 20240828151850.png]]
Y ya podremos volver a escribir en el disco:
![[Pasted image 20240828151910.png]]
Ahora si queremos que cuando se reinicie el sistema se mantenga el acceso a la partición protegida, tenemos que editar el archivo `/etc/fstab` y añadir la siguiente línea:
```bash
/dev/mapper/sdb1_encriptada /mnt/particion_encriptada ext4 errors=remount-ro 0 2
```
![[Pasted image 20240828152250.png]]
Y ahora tras reiniciar el sistema, se nos pide que introduzcamos la contraseña para acceder al disco:
![[Pasted image 20240828152404.png]]
Tras poner la contraseña 123123, que es la que hemos configurado, podremos acceder al sistema:
