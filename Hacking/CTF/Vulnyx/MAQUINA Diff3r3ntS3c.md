Hacemos el escaneo de nmap:
![[Pasted image 20240406115109.png]]
Y esto es lo que corre por el puerto 80:
![[Pasted image 20240406115223.png]]
Pero dentro del apartado get in touch vemos un lugar para subir archivos:
![[Pasted image 20240406115232.png]]
Intentamos enviar un .php malicioso pero vemos que no podemos porque se debe estar aplicando algún filtro para subir archivos:
![[Pasted image 20240406120254.png]]
Probamos en interceptar la petición con burp suite y cambiar la extensión a phtml:
![[Pasted image 20240406120358.png]]
Y probamos en subir la reverse shell con .phtml:
![[Pasted image 20240406120747.png]]
Donde vemos que la acepta:
![[Pasted image 20240406120825.png]]
Ahora si hacemos fuzzing web encontramos varios directorios, donde vemos /uploads:
![[Pasted image 20240406120903.png]]
Y nos encontramos con los siguientes directorios:
![[Pasted image 20240406120922.png]]
Vemos que en varias de estas carpetas obtenemos la reverse shell:
![[Pasted image 20240406121052.png]]
Y recibimos la reverse shell:
![[Pasted image 20240406121105.png]]
Vamos a ver los procesos del sistema con pspy64:
```bash
https://github.com/DominicBreuker/pspy/releases
```
![[Pasted image 20240406121314.png]]
Y vemos que se ejecuta un script llamado makeBackup.sh:
![[Pasted image 20240406121602.png]]
Le añadimos el siguiente comando para que cambie permisos a la bash:
![[Pasted image 20240406121656.png]]
Y ya somos root:
![[Pasted image 20240406121720.png]]
