Lo primero tenemos que tener fastboot instalado:
```bash
apt install fastboot
```
![[Pasted image 20240524144750.png]]
El siguiente paso será tener un recovery y una ROM:
![[Pasted image 20240524144825.png]]
Entramos en modo fastboot en el Android y lo conectamos al PC, donde ejecutamos el siguiente comando:
```bash
fastboot flash recovery recovery.img
fastboot boot recovery.img
```
![[Pasted image 20240524165923.png]]
Y ahora instalamos la ROM:
![[Pasted image 20240524165941.png]]
