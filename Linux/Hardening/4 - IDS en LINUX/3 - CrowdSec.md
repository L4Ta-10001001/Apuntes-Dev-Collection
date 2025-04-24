CrowdSec es una herramienta moderna de ciberseguridad basada en la detección y mitigación de comportamientos maliciosos. Funciona mediante el análisis de logs y eventos, y es capaz de bloquear IPs maliciosas, además de compartir esta información con una red colaborativa. Esta es su web donde debemos registrarnos:
![[Pasted image 20240905100018.png]]


-------

Para instalarlo, ejecutaremos los siguientes comandos:
```bash
sudo apt install curl sudo gnupg lsb-release
curl -s https://install.crowdsec.net | sudo sh
sudo apt install crowdsec
sudo apt install crowdsec-firewall-bouncer-iptables
```
Después tenemos que ejecutar este comando para vincular crowdsec de mi sistema con la interfaz web:
```bash
sudo cscli console enroll cm0ozyelr0000oscvcdznu56b
```
![[Pasted image 20240905100710.png]]
![[Pasted image 20240905100722.png]]
Una vez instalado, podemos ver los distintos escenarios que ya vienen preconfigurados:
![[Pasted image 20240905100847.png]]
### ESCENARIOS
Los escenarios son configuraciones específicas que definen qué tipo de comportamiento malicioso debe detectar CrowdSec (por ejemplo, intentos de fuerza bruta, escaneo de puertos, etc.). Puedes listar los escenarios instalados con el siguiente comando:
```bash
sudo cscli scenarios list
```
![[Pasted image 20240905093618.png]]
Ahora si por ejemplo queremos crear un nuevo escenario para auditar intentos de fuerza bruta por ssh, lo hacemos de la siguiente forma:
```bash
sudo cscli scenarios install crowdsecurity/ssh-bf
```
![[Pasted image 20240905093729.png]]
Ahora si queremos configurar este escenario, editamos el siguiente archivo:
```bash
sudo nano /etc/crowdsec/scenarios/ssh-bf.yaml
```
