Vamos a file -> Project structure:
![[Pasted image 20240520130322.png]]
Nos ubicamos en Artifacts:
![[Pasted image 20240520130342.png]]
Luego vamos a la categoría de Build -> Build Artifacts:
![[Pasted image 20240520130401.png]]
Y hacemos clic en build:
![[Pasted image 20240520130433.png]]
Donde vemos que se nos crea una carpeta llamada out/artifacts y el nombre del proyecto, donde lo tendremos en formato .jar:
![[Pasted image 20240520130512.png]]
Lo tenemos aquí mismo:
![[Pasted image 20240520130542.png]]
Y con el comando java -jar lo ejecutamos:
```java
java -jar .\pruebas.jar
```
![[Pasted image 20240520130603.png]]
Si nos diera algún error, podemos instalar el JDK Development Kit:
![[Pasted image 20240520130635.png]]
Ahora debemos de usar una aplicación llamada launch4j para convertir este .jar en .exe:
![[Pasted image 20240520151713.png]]
Donde tendremos que especificar primero el .jar y el .exe resultante:
![[Pasted image 20240520151928.png]]
Y ahora hay que poner la versión de JRE:
![[Pasted image 20240520151811.png]]
Para conocer esta versión, ejecutaremos el siguiente comando, donde vemos la versión 22:
```bash
java --version
```
![[Pasted image 20240520151839.png]]
Por lo que ponemos 22:
![[Pasted image 20240520151909.png]]
Y ya lo tenemos:
![[Pasted image 20240520152018.png]]
