Hacemos el escaneo de nmap:
![[Pasted image 20240201113455.png]]
Y esto corre por el puerto 80:
![[Pasted image 20240201113712.png]]
Si hacemos clic en login as guest vemos que nos redirige a esta página:
![[Pasted image 20240201113738.png]]
Si hacemos clic en el fichero adjunto de Attachment del usuario Hazard, vemos que se trata de un archivo de configuración de cisco:
![[Pasted image 20240201113938.png]]
Estas contraseñas las podemos crackear en la siguiente url:
```bash
https://www.ifm.net.nz/cookbooks/passwordcracker.html
```
![[Pasted image 20240201114126.png]]
![[Pasted image 20240201114150.png]]
Las contraseñas son las siguientes:
```bash
rout3r:$uperP@ssword
admin:Q4)sJu\Y8qz*A3?d
```
Y también vemos el secret:
```bash
$1$pdQG$o8nrSzsGXeaduXrjlvKc91
```
![[Pasted image 20240201114315.png]]
Y obtenemos la siguiente contraseña al crackear este hash con john the ripper:
```
stealth1agent
```
![[Pasted image 20240201114427.png]]
Probamos a ver si esta contraseña es del usuario hazard, pero vemos que no, ya que podemos enumerar recursos compartidos haciendo uso de una null session pero no de forma autenticada:
![[Pasted image 20240201114703.png]]
Pero con impacket utilizando lookupsid podemos hacer fuerza bruta de usuarios y grupos:
```bash
impacket-lookupsid hazard:stealth1agent@10.10.10.149
```
![[Pasted image 20240201115916.png]]
Podemos probar con cada uno de estos usuarios acceder mediante evil-winrm con la contraseña que hemos encontrado antes de administrador; y vemos que con el usuario chase funciona:
```bash
evil-winrm -u chase -p "Q4)sJu\Y8qz*A3?d" -i 10.10.10.149
```
![[Pasted image 20240201120343.png]]


