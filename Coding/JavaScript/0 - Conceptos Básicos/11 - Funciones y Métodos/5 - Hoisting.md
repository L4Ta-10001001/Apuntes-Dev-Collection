**Hoisting** en JavaScript es un comportamiento por el cual las declaraciones de variables y funciones se "mueven" al principio de su contexto de ejecución antes de que el código sea ejecutado.
```javascript
console.log(coche);

var coche = "ferrari";
```
Vemos que nos muestra undefined, pero no saca un error:
![[Pasted image 20241015085155.png]]
Pero en cambio se declaramos la veriable con let, será más estricto y aquí sí nos dará error:
```javascript
console.log(coche);

let coche = "ferrari";
```
![[Pasted image 20241015085249.png]]
