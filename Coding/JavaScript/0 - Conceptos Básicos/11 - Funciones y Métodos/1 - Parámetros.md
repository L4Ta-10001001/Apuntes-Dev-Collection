**Parámetros Básicos**
Puedes definir una función con uno o más parámetros, y al llamar a la función, puedes pasar los argumentos correspondientes.
```javascript
function saludar(nombre) {
    console.log(`Hola, ${nombre}!`);
}

saludar('Mario'); // Salida: Hola, Mario!
```
**Múltiples Parámetros**
Puedes definir múltiples parámetros en una función separados por comas.
```javascript
function sumar(a, b) {
    return a + b;
}

console.log(sumar(3, 4)); // Salida: 7
```
![[Pasted image 20241014123042.png]]

**Parámetros Opcionales**
Puedes proporcionar un valor predeterminado para un parámetro. Si no se pasa un argumento para ese parámetro, se usará el valor predeterminado.
```javascript
function saludar(nombre = 'Invitado') {
    console.log(`Hola, ${nombre}!`);
}

saludar('Mario'); // Salida: Hola, Mario!
saludar(); // Salida: Hola, Invitado!
```
![[Pasted image 20241014123054.png]]
**Parámetros Rest**
El operador de propagación `...` se puede usar para manejar un número variable de argumentos. Esto se llama "parámetro rest".
```javascript
function sumarTodos(...numeros) {
    return numeros.reduce((acum, num) => acum + num, 0);
}

console.log(sumarTodos(1, 2, 3, 4)); // Salida: 10
console.log(sumarTodos(5, 10, 15)); // Salida: 30
```
![[Pasted image 20241014123105.png]]
**Parámetros por Nombre (Desestructuración)**
Si pasas un objeto como argumento, puedes usar la desestructuración para obtener propiedades específicas.
```javascript
function mostrarPersona({ nombre, edad }) {
    console.log(`Nombre: ${nombre}, Edad: ${edad}`);
}

const persona = { nombre: 'Mario', edad: 30 };
mostrarPersona(persona); // Salida: Nombre: Mario, Edad: 30
```
![[Pasted image 20241014123115.png]]
**Parámetros con Funciones**
También puedes pasar funciones como parámetros a otras funciones.
```javascript
function operar(a, b, operacion) {
    return operacion(a, b);
}

const suma = (x, y) => x + y;
const resta = (x, y) => x - y;

console.log(operar(5, 3, suma)); // Salida: 8
console.log(operar(5, 3, resta)); // Salida: 2
```
![[Pasted image 20241014123126.png]]
# Parámetro REST y Operador SPREAD

### REST
El **operador rest** en JavaScript se utiliza para agrupar un número variable de argumentos en un array. Se representa con tres puntos (`...`) y se puede usar en la declaración de funciones para capturar múltiples argumentos como un solo array. Esto es especialmente útil cuando no se sabe de antemano cuántos argumentos se pasarán a la función.
```javascript
function sumarTodos(...numeros) {
    return numeros.reduce((acumulador, num) => acumulador + num, 0);
}

console.log(sumarTodos(1, 2, 3)); // Salida: 6
console.log(sumarTodos(5, 10, 15, 20)); // Salida: 50
```
![[Pasted image 20241014123856.png]]
También podemos combinar parámetros regulares con parámetros rest. En este caso, los parámetros rest deben ir al final de la lista de parámetros.
```javascript
function mostrarInformacion(nombre, edad, ...hobbies) {
    console.log(`Nombre: ${nombre}`);
    console.log(`Edad: ${edad}`);
    console.log(`Hobbies: ${hobbies.join(', ')}`);
}

mostrarInformacion('Mario', 30, 'programar', 'leer', 'jugar videojuegos');
// Salida:
// Nombre: Mario
// Edad: 30
// Hobbies: programar, leer, jugar videojuegos
```
![[Pasted image 20241014123946.png]]

## SPREAD
El **operador spread** en JavaScript, que también se representa con tres puntos (`...`), se utiliza para expandir o descomponer elementos de un array u objeto en lugares donde se esperan múltiples elementos. Es especialmente útil para copiar y combinar arrays, pasar argumentos a funciones y desestructurar objetos. A continuación, te muestro cómo se utiliza el operador spread con ejemplos prácticos.
**Combinar Arrays**
Puedes combinar varios arrays en uno solo utilizando el operador spread.
```javascript
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const combinado = [...array1, ...array2];

console.log(combinado); // Salida: [1, 2, 3, 4, 5, 6]
```
![[Pasted image 20241014124141.png]]
**Copiar Arrays**
También puedes crear una copia de un array utilizando el operador spread.
```javascript
const original = [1, 2, 3];
const copia = [...original];

console.log(copia); // Salida: [1, 2, 3]
copia.push(4);
console.log(original); // Salida: [1, 2, 3] (el array original no se modifica)
```
![[Pasted image 20241014124250.png]]
**Combinar Objetos**
Puedes combinar objetos utilizando el operador spread, creando un nuevo objeto que contiene las propiedades de ambos.
```javascript
const objeto1 = { a: 1, b: 2 };
const objeto2 = { b: 3, c: 4 };
const combinado = { ...objeto1, ...objeto2 };

console.log(combinado); // Salida: { a: 1, b: 3, c: 4 }
```
![[Pasted image 20241014124327.png]]
