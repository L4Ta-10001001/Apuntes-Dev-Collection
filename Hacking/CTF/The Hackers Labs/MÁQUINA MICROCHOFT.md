Este sería el escaneo de nmap:
![[Pasted image 20240404103835.png]]
Vemos abierto el puerto 445, por lo que vamos a utilizar el script vuln de nmap para ver si es vulnerable:
```bash
nmap -p445 -sS --script='vuln'  -vvv -Pn 192.168.0.25
```
Y vemos que sí:
![[Pasted image 20240404104050.png]]
Buscamos este exploit en metasploit y lo encontramos:
![[Pasted image 20240404104122.png]]
Utilizaremos el primero de ellos, donde obtenemos acceso:
```bash
use windows/smb/ms17_010_eternalblue
```
![[Pasted image 20240404104223.png]]
Y ya somos el usuario administrador:
![[Pasted image 20240404104502.png]]
