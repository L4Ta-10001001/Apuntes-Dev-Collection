Nos encuentra estos puertos abiertos:
![[Pasted image 20240416114426.png]]
![[Pasted image 20240416114444.png]]
![[Pasted image 20240416114453.png]]
Para conectarnos por RDP tenemos que ejecutar el siguiente comando usando la herramienta xfreerdp:
```bash
xfreerdp /u:Administrator /p:letmein123! /v:10.10.142.238:3389
```
![[Pasted image 20240416114636.png]]
![[Pasted image 20240416114643.png]]
Si vamos a configuración y a la parte de about, vemos la versión de windows:
![[Pasted image 20240416114852.png]]
Ahora para saber qué usuarios se han logueado previamente, tenemos que abrir el event viewer, yendo a la parte de windows logs y viendo que el usuario administrator fue el último en loguearse:
![[Pasted image 20240416115129.png]]
Si nos preguntan información de un usuario, por ejemplo cuando fue la última vez que inició sesión, debemos de usar el comando net user john:
![[Pasted image 20240416115430.png]]
Para enumerar más cuentas, usamos el comando net user:
![[Pasted image 20240416115637.png]]
Si nos preguntan por las cuentas con permisos de administrador, usamos el comando net localgroup Administrators:
![[Pasted image 20240416115919.png]]
