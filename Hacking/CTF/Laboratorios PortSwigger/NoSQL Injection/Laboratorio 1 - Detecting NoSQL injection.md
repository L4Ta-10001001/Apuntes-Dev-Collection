Tenemos el siguiente laboratorio:
![[Pasted image 20240325134910.png]]
Si vamos a la categoría de gifts, vemos que en la url se establece un category=gifts:
![[Pasted image 20240325135327.png]]
Y si colamos una comilla, se nos muestra el siguiente mensaje de error donde vemos que estamos ante una base de datos MondoDB:
![[Pasted image 20240325135402.png]]
Viendo esto, podemos establecer un operador lógico OR con Gifts'||1||' y se nos muestra todo el contenido de la base de datos:
![[Pasted image 20240325135554.png]]
