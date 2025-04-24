En JavaScript, el bucle `do while` se utiliza para ejecutar un bloque de código al menos una vez incluso cuando la condición no se cumpla, tal y como ocurre en este caso que no entraríamos dentro del bucle, pero aún así lo que está dentro del do se ejecutará:
```javascript
let numeros = 47;

do{
    console.log(numeros);

    numeros--;
}while(numeros > 77);
```
![[Pasted image 20241014121414.png]]
