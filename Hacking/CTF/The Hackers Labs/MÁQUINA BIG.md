Antes de nada tenemos que crear una red en virtualbox:
![[Pasted image 20240514175154.png]]
Y ahora ya podremos arrancarla, por lo que hacemos el escaneo de nmap:
![[Pasted image 20240514175847.png]]
Esto es lo que corre por el puerto 80:
![[Pasted image 20240514180146.png]]
Viendo el código fuente nos encontramos con el usuario juicy:
![[Pasted image 20240514180219.png]]
Haciendo fuzzing web, nos encontramos con los siguientes directorios:
![[Pasted image 20240514180409.png]]
De esta imágenes, la big2 esconde algo:
![[Pasted image 20240514180436.png]]
Para entrar por evil-winrm usamos las siguientes credenciales:
```bash
evil-winrm -i bbr.thl -u Juicy -p Bigpoppa1972
```
![[Pasted image 20240723090415.png]]
Ahora ejecutamos los siguientes comandos para enumerar permisos del usuario:
```bash
net users
net groups
```
![[Pasted image 20240723090509.png]]
Para enumerar, usaremos sharphound:
```bash
https://github.com/BloodHoundAD/SharpHound/releases?page=2
```
Y subiremos sharphound.exe:
![[Pasted image 20240723090726.png]]
Una vez dentro, lo ejecutamos usando el comando ./SharpHound.exe:
![[Pasted image 20240723090945.png]]
Nos descargaremos el .zip resultante:
![[Pasted image 20240723091024.png]]
Y ahora tenemos que bajarnos este archivo y leerlo con bloodhound:



