Un array es una estructura de datos que permite almacenar múltiples valores en una sola variable. Los elementos de un array pueden ser de cualquier tipo, incluidos números, cadenas, objetos y otros arrays.
```javascript
let frutas = ['manzana', 'naranja', 'plátano'];
console.log(frutas[0]); // 'manzana'
console.log(frutas[1]); // 'naranja'
```
**Métodos comunes**
JavaScript proporciona varios métodos útiles para trabajar con arrays:
**`push()`**: Agrega uno o más elementos al final del array.
```javascript
frutas.push('kiwi'); // ['manzana', 'naranja', 'plátano', 'kiwi']
```
**`pop()`**: Elimina el último elemento del array y lo devuelve.
```javascript
let ultimo = frutas.pop(); // 'kiwi'
```
**`shift()`**: Elimina el primer elemento del array y lo devuelve.
```javascript
let primero = frutas.shift(); // 'manzana'
```
**`unshift()`**: Agrega uno o más elementos al principio del array.
```javascript
frutas.unshift('fresa'); // ['fresa', 'naranja', 'plátano']
```
**`slice()`**: Devuelve una copia superficial de una porción del array.
```javascript
let citricas = frutas.slice(1, 3); // ['naranja', 'plátano']
```
**`forEach()`**: Ejecuta una función para cada elemento del array.
```javascript
frutas.forEach(function(fruta) {
    console.log(fruta);
});
```
**`map()`**: Crea un nuevo array con los resultados de aplicar una función a cada elemento.
```javascript
let longitudes = frutas.map(function(fruta) {
    return fruta.length;
});
```
**`filter()`**: Crea un nuevo array con todos los elementos que pasen una prueba.
```javascript
let frutasLargas = frutas.filter(function(fruta) {
    return fruta.length > 6;
});
```

------------

Podemos ver un ejemplo completo:
```javascript
let frutas = ['manzana', 'naranja', 'plátano'];

// Agregar una fruta
frutas.push('kiwi');

// Recorrer el array
frutas.forEach(function(fruta) {
    console.log(fruta);
});

// Filtrar frutas con más de 6 letras
let frutasLargas = frutas.filter(fruta => fruta.length > 6);
console.log(frutasLargas); // ['naranja', 'plátano']
```
![[Pasted image 20241015195923.png]]
