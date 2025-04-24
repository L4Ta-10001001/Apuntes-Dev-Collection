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
