Hacemos el escaneo de nmap:
![[Pasted image 20240303123051.png]]
Y esto corre por el puerto 80:
![[Pasted image 20240303123014.png]]
Y si lanzamos un whatweb vemos que se trata de un servidor IIS:
![[Pasted image 20240303123139.png]]
Si intentamos subir un archivo malicioso aspfx vemos que no lo permite:
![[Pasted image 20240303123336.png]]
Pero en cambio si le cambiamos el nombre a la extensión .theme, vemos que sí funciona:
![[Pasted image 20240303123841.png]]
![[Pasted image 20240303123857.png]]
Y si hacemos fuzzing nos encuentra el directo upload:
![[Pasted image 20240303123510.png]]
Comprobamos que existe:
![[Pasted image 20240303123524.png]]
Llegados a este punto, podemos buscar temas de windows 11 que contengan exploits y así tratar de subirlo, donde nos encontramos con el CVE-2023-38146:
![[Pasted image 20240303124024.png]]
