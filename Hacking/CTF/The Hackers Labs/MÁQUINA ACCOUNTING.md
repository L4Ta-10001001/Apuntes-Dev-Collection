Hacemos el escaneo con nmap:
![[Pasted image 20240809132917.png]]
![[Pasted image 20240809132928.png]]
Por el puerto 9081 vemos que hay una web, por lo que la visualizamos y obtenemos información con whatweb:
![[Pasted image 20240809133159.png]]
![[Pasted image 20240809133227.png]]
Realizamos fuzzing web donde encontramos un archivo llamado notas.txt donde obtenemos las credenciales supervisor:supervisor en http://192.168.0.26:9081/download/notas.txt
![[Pasted image 20240809133738.png]]
![[Pasted image 20240809133855.png]]
Con estas credenciales podemos acceder a la web, pero una vez dentro no hay forma de obtener intrusión al servidor, por lo que probamos en enumerar recursos compartidos:
```bash
smbclient --no-pass -L //192.168.0.26
```
![[Pasted image 20240809134535.png]]
Accedemos al recurso compartido Compac y nos encontramos con dos carpetas, donde en la de Empresas vemos un SQL.txt:
```bash
smbclient -N \\\\192.168.0.26\\Compac
```
Nos descargamos este archivo y obtenemos unas credenciales:
```bash
sa:Contpaqi2023.
```
![[Pasted image 20240809134749.png]]
Y vemos que por el puerto 49992 corría una base de datos:
![[Pasted image 20240809134939.png]]
Por lo que podemos acceder a esta base de datos utilizando el cliente de impacket, está diseñado específicamente para interactuar con bases de datos Microsoft SQL Server. No es compatible con otras bases de datos como Oracle, MySQL, PostgreSQL, etc.
```bash
impacket-mssqlclient COMPAC/sa:Contpaqi2023.@192.168.0.26 -port 49992
```
Y una vez dentro podremos ejecutar comandos:
![[Pasted image 20240809135527.png]]
Incluso una vez dentro podemos ejecutar comandos para interactuar con la base de datos:
```bash
SELECT name FROM sys.databases;
USE nombre_base de datos
```
![[Pasted image 20240809140742.png]]
