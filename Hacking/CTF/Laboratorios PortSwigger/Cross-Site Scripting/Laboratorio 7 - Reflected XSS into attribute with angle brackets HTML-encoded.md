Tenemos el siguiente laboratorio:
![[Pasted image 20240524174847.png]]
Dentro del buscador ponemos cualquier dato:
![[Pasted image 20240524175026.png]]
Y por aquí vemos que lo que ponemos en el buscador se refleja arriba:
![[Pasted image 20240524175406.png]]
Teniendo en cuenta los dos últimos puntos, podemos crear un payload que nos cree un nuevo atributo dentro del elemento input para que se nos ejecute un alert. En este caso el payload es:
```bash
'"onmousemove="alert(1)'
```
![[Pasted image 20240524175618.png]]
