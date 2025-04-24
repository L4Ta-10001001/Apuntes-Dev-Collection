Primero debemos de instalar la interfaz gráfica:
```bash
sudo apt install kali-win-kex
```
Y luego la arrancamos o detenemos con los siguientes comandos:
```bash
kex --win
kex --win stop
```
# CONFIGURAR REDES
Para que Kali Linux en WSL (Windows Subsystem for Linux) use la misma interfaz de red que la máquina anfitriona, puedes seguir estos pasos. Esto es útil para que WSL pueda comunicarse directamente con otros dispositivos en la misma red o para usar herramientas de red de Kali Linux de manera más eficiente.
```bash
netsh interface portproxy add v4tov4 listenport=80 listenaddress=0.0.0.0 connectport=80 connectaddress=<ip_de_wsl>
```
Y para ver los puertos reenviados:
```bash
netsh interface portproxy show all
```
![[Pasted image 20240921084900.png]]
Y para eliminar esta regla, lo hacemos así:
```bash
netsh interface portproxy delete v4tov4 listenport=80 listenaddress=0.0.0.0
```
# HACER BACKUPS DE WSL
Primero, verifica el nombre exacto de tu distribución de **Kali Linux** en WSL. Para hacerlo, abre una ventana de **PowerShell** y ejecuta:
```bash
wsl --list
```
![[Pasted image 20240921085623.png]]
Para hacer el backup de tu instalación de **Kali Linux**, utiliza el siguiente comando en **PowerShell** (ejecutado como administrador):
```bash
wsl --export kali-linux kali_backup.tar
```
![[Pasted image 20240921090248.png]]
Ahora vamos a probar en restaurarla, donde primero eliminaremos la instalación anterior:
```bash
wsl --unregister kali-linux
```
![[Pasted image 20240921090338.png]]
Y ahora vamos a importarlo:
```bash
wsl --import nuevo-kali C:\Users\mario\Documents\kali_wsl kali_backup.tar
```
![[Pasted image 20240921090543.png]]
Ahora para lanzar este kali, ejecutamos el siguiente comando:
```bash
wsl -d nuevo-kali
```
Pero vamos a tener el siguiente problema, y es que se montará de tal forma que acceda a todos los archivos de windows, por lo que tenemos que editar el archivo /etc/wsl.conf y añadir las siguientes líneas:
```bash
[automount]
enabled = false
```
![[Pasted image 20240921091159.png]]
Reiniciamos wsl:
```bash
wsl --shutdown
```
Y ya tenemos el kali aislado:
![[Pasted image 20240921091232.png]]
