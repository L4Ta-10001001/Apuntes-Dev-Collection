Tenemos el siguiente laboratorio:
![[Pasted image 20240507181048.png]]
Si entramos en alguna de las categorías, vemos que se hace una query por detrás, fijándonos en la url:
![[Pasted image 20240507181215.png]]
En este punto lo que hay que hacer es adivinar el número de columnas de la tabla, donde cada NULL representa una columna, y por ejemplo poniendo 3 NULL, acertamos con que la tabla tiene 3 columnas y por tanto se imprime toda la información:
```bash
/filter?category=Accessories+UNION+SELECT+NULL,NULL,NULL--
```
![[Pasted image 20240507181449.png]]