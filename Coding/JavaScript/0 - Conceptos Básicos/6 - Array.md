Los arrays en JavaScript son objetos especiales que permiten almacenar colecciones de datos, pudiendo crear un array utilizando la notación de corchetes `[]`:
```javascript
const frutas = ["manzana", "banana", "naranja"];
```
Y luego podemos acceder a estos elementos con un console.log:
```javascript
console.log(frutas[0]); // manzana
console.log(frutas[1]); // banana
```
Y podemos modificarlo de la siguiente forma:
```javascript
frutas[2] = "kiwi";
console.log(frutas); // ["manzana", "banana", "kiwi"]
```
