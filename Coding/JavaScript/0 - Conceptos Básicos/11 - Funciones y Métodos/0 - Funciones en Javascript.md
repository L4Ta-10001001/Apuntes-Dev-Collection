Las funciones en JavaScript son bloques de código reutilizables que realizan una tarea específica. Se pueden definir de varias maneras, y se utilizan para estructurar el código de manera más eficiente y organizada, existiendo los distintos tipos:
**Función Declarativa**
```javascript
function saludar(nombre) {
    return `Hola, ${nombre}!`;
}

console.log(saludar('Mario')); // Salida: Hola, Mario!
```
![[Pasted image 20241014122620.png]]
**Función Expresiva**
```javascript
const saludar = function(nombre) {
    return `Hola, ${nombre}!`;
};

console.log(saludar('Mario')); // Salida: Hola, Mario!
```
![[Pasted image 20241014122620.png]]

**Funciones Flecha**
```javascript
const saludar = (nombre) => {
    return `Hola, ${nombre}!`;
};

console.log(saludar('Mario')); // Salida: Hola, Mario!

// Versión más corta si solo hay una línea de código
const saludarCorta = nombre => `Hola, ${nombre}!`;

console.log(saludarCorta('Mario')); // Salida: Hola, Mario!
```
![[Pasted image 20241014122620.png]]


