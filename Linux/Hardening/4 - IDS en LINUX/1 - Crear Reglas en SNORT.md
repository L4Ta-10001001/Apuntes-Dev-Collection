Para crear reglas con snort tenemos que editar el archivo `/etc/snort/rules/local.rules`:
![[Pasted image 20240829234211.png]]
Las reglas en Snort siguen un formato específico. Aquí hay un ejemplo básico de una regla de Snort:
```bash
alert icmp any any -> 192.168.122.209 any (msg:"Ping detectado"; sid:1000001;)
```
![[Pasted image 20240829235046.png]]
Ahora, si lanzamos un PING dese otra máquina a esta, se habrá generado un log con el registro de dicho ping:
![[Pasted image 20240829235125.png]]
Teniendo un archivo llamado `/var/log/snort` que podremos visualizarlo con el siguiente comando:
```bash
u2spewfoo snort.log
```
![[Pasted image 20240829235225.png]]
Aunque si queremos que el reporte esté en forma de log más sencillo, podemos editar el archivo `etc/snort/snort.conf`descomentando la línea señalada y comentando la otra:
![[Pasted image 20240829235913.png]]
Y ahora podemos visualizar el archivo `/var/log/auth.log` y vemos nuestra regla creada:
```bash
tail /var/log/auth.log
```
![[Pasted image 20240830000332.png]]
### EJEMPLO CON SSH
Añadimos la siguiente regla al archivo local.rules:
```bash
alert tcp any any -> 192.168.122.209 22 (msg:"Intento de inicio de sesión por SSH detectado"; sid:1000002; rev:1;)
```
![[Pasted image 20240830000900.png]]
Ahora desde otra máquina nos conectamos por SSH:
![[Pasted image 20240830000923.png]]
Y si visualizamos el archivo `/var/log/auth.log` podremos ver nuestra alerta:
![[Pasted image 20240830001019.png]]
