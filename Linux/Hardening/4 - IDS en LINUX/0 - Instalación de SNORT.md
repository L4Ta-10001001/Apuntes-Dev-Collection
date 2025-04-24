Snort es un sistema de detección de intrusiones (IDS, por sus siglas en inglés) de código abierto que se utiliza en sistemas Linux (y otros sistemas operativos) para monitorear y analizar el tráfico de red en tiempo real. Snort tiene la capacidad de detectar una amplia gama de ataques y actividades sospechosas, lo que lo convierte en una herramienta esencial para la seguridad informática.

----

Para instalarlo ejecutamos el comando `apt install snort` donde primero debemos consultar nuestra IP:
![[Pasted image 20240829233229.png]]
Y ahora podremos introducir el rango de red que queramos abarcar con snort:
![[Pasted image 20240829233251.png]]
Y ahora snort estará en funcionamiento, de tal forma que si ejecutamos el siguiente comando, vemos como snort se ha lanzado cogiendo nuestra interfaz de red enp0s3 y nuestro rango de red:
![[Pasted image 20240829233546.png]]
Podemos también consultar las reglas en total:
![[Pasted image 20240829233743.png]]
