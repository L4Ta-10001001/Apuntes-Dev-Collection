Tenemos el siguiente laboratorio:
![[Pasted image 20240527113935.png]]
Al buscar por algo en el campo de búsqueda, vemos como la palabra que buscamos se encuentra dentro del código fuente dentro de javascript:
![[Pasted image 20240527114059.png]]
Podemos probar con el siguiente payload, pero no va a funcionar porque javascript no permite espacios:
```bash
' alert('XSS') '
```
![[Pasted image 20240527114452.png]]
Para hacer un bypass de esto, podemos usar guiones:
```bash
'-alert("XSS")-'
```
![[Pasted image 20240527114612.png]]
Y ya habremos completado el laboratorio:
![[Pasted image 20240527114631.png]]
