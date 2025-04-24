## SetUID
Permite que un programa se ejecute con los privilegios del propietario del archivo, en lugar de los privilegios del usuario que lo está ejecutando. Esto es especialmente útil para programas que necesitan realizar tareas que requieren permisos elevados, como cambiar la contraseña de un usuario.

----

Podemos encontrarlos con el comando find:
```bash
find / -type f -perm /4000 2>/dev/null | xargs ls -l
```
![[Pasted image 20241002091953.png]]
Este permiso se otorga con el comando chmod u+s (en caso de ser a un usuario)
## SetGID



## Sticky Bit
El **sticky bit** es un permiso especial en sistemas Unix y Linux que se utiliza principalmente en directorios. Su propósito es proteger los archivos dentro de un directorio compartido, permitiendo que solo el propietario de un archivo pueda borrarlo o renombrarlo, independientemente de los permisos del directorio.

Este permiso se utiliza por ejemplo en el directorio /tmp y se representa con una t:
![[Pasted image 20241002092400.png]]
Pudiendo otorgarlo o quitarlo con el siguiente comando:
```bash
sudo chmod +t /tmp
```


