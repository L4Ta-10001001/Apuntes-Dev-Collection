Tenemos el siguiente laboratorio:
![[Pasted image 20240404104608.png]]
Y hacemos clic en alguno de los productos, donde podemos comprobar su disponibilidad:
![[Pasted image 20240404104833.png]]
E interceptamos la petición con burp suite:
![[Pasted image 20240404104921.png]]
Cambiaremos la petición para hacer la solicitud al directorio /admin:
![[Pasted image 20240404105016.png]]
Y así en la web se nos muestra lo siguiente:
![[Pasted image 20240404105030.png]]
Ahora debemos de mirar el código fuente para ver el href del usuario Carlos:
![[Pasted image 20240404105216.png]]
Una vez visto, podemos realizar una nueva petición para ordenar al servidor que borre a dicho usuario:
```bash
http://localhost/admin/delete?username=carlos
```
![[Pasted image 20240404105705.png]]
Y al enviar esta petición ya habremos resuelto el laboratorio:
![[Pasted image 20240404105732.png]]
