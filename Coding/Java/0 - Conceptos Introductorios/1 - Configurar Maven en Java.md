Maven administra las dependencias del proyecto, lo que significa que puede descargar automáticamente las bibliotecas y dependencias requeridas por tu proyecto desde repositorios remotos como Maven Central Repository.
![[Pasted image 20240319112230.png]]
Lo metemos en una carpeta donde nos acordemos, como en la ruta donde está java instalado:
![[Pasted image 20240319112249.png]]
Entramos dentro de la carpeta y copiamos su ruta:
![[Pasted image 20240319112330.png]]
Vamos a la parte de editar variables del sistema:
![[Pasted image 20240319112426.png]]
Vamos donde pone variables de entorno:
![[Pasted image 20240319112441.png]]
Creamos una nueva variable de entorno llamada maven donde pegaremos la ruta:
![[Pasted image 20240319112504.png]]
![[Pasted image 20240319112541.png]]
Y ahora vamos a editar el path:
![[Pasted image 20240319112613.png]]
Y lo añadimos al path:
![[Pasted image 20240319112644.png]]
Y validamos que todo esté correcto imprimiendo la variable de entorno que acabamos de crear:
![[Pasted image 20240319112818.png]]
![[Pasted image 20240319112755.png]]
Ahora vamos a crear un nuevo proyecto en java:
![[Pasted image 20240319113600.png]]
Seleccionamos Maven y luego maven-archetype-quickstart:
![[Pasted image 20240319113645.png]]
Nos pedirá una carpeta donde guardar el proyecto, y después nos saldrá esto donde tendremos que hacer clic en enter:
![[Pasted image 20240319114135.png]]
Abrimos la carpeta:
![[Pasted image 20240319114214.png]]
Y ya tenemos nuestro proyecto preparado:
![[Pasted image 20240319114226.png]]
Y ahora si queremos añadir cualquier dependencia, vamos a Maven repository y añadimos la que queramos. Por ejemplo la de MySQL connector:
![[Pasted image 20240319114354.png]]
Y lo pegamos en el archivo pom.xml:
![[Pasted image 20240319114615.png]]
Y ahora si ejecutamos el código de por ejemplo una conexión a la base de datos, veremos que funciona gracias a que hemos añadido la dependencia:
![[Pasted image 20240319114728.png]]
![[Pasted image 20240319114736.png]]
