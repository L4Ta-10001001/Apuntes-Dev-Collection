Hacemos el escaneo de nmap:
![[Pasted image 20240730130748.png]]
Y así es la web:
![[Pasted image 20240730130802.png]]
Vemos un panel de hacer reservas:
![[Pasted image 20240730130831.png]]
Interceptamos la petición, pero vemos que dependiendo de si enviamos un número o un texto vemos un error diferente:
![[Pasted image 20240730131343.png]]
![[Pasted image 20240730131355.png]]
Si probamos solo con números nos dice algo de base64:
![[Pasted image 20240730131609.png]]
Por lo que intentamos colar un comando en base64 y vemos que funciona:
![[Pasted image 20240730131709.png]]
Una vez dentro, vemos que en /com/perosnal/hash.lst tenemos distintos hashes:
![[Pasted image 20240730131936.png]]
Para creackear estos hashes, usaremos la siguiente herramienta:
https://github.com/PatxaSec/SHA_Decrypt
![[Pasted image 20240730132703.png]]
Con esta herramienta la ejecutamos de la siguiente forma:
```python
python3 script.py 'd' '$SHA1$d$BjkVArB9RcGUs3sgVKyAvxzH0eA=' '/usr/share/wordlists/rockyou.txt'
```
Donde en este caso le pasamos el salto d, el cual en el contexto de hashes, especialmente cuando se trata de hashes de contraseñas, "salto" se refiere a un valor aleatorio que se utiliza para hacer que cada hash sea único, incluso si la misma contraseña se utiliza en diferentes ocasiones. Este valor ayuda a proteger contra ataques de diccionario y ataques de fuerza bruta, porque hace que el hash de una contraseña específica sea diferente cada vez que se genera.

Y nos habrá encontrado la contraseña de root:
![[Pasted image 20240730133047.png]]
