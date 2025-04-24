Los operadores de comparación en JavaScript se utilizan para comparar dos valores y devolver un resultado booleano (`true` o `false`).
```javascript
let a = 5;
let b = 10;

// Operadores de comparación
console.log(`a = ${a}, b = ${b}`);

// Igualdad (==)
console.log(`Igualdad (a == b): ${a == b}`); // false

// Igualdad estricta (===)
console.log(`Igualdad estricta (a === b): ${a === b}`); // false

// Desigualdad (!=)
console.log(`Desigualdad (a != b): ${a != b}`); // true

// Desigualdad estricta (!==)
console.log(`Desigualdad estricta (a !== b): ${a !== b}`); // true

// Mayor que (>)
console.log(`Mayor que (a > b): ${a > b}`); // false

// Mayor o igual que (>=)
console.log(`Mayor o igual que (a >= b): ${a >= b}`); // false

// Menor que (<)
console.log(`Menor que (a < b): ${a < b}`); // true

// Menor o igual que (<=)
console.log(`Menor o igual que (a <= b): ${a <= b}`); // true

// Comparaciones con valores iguales
let x = '5';
console.log(`x = ${x}`);

// Igualdad (==) con coerción de tipo
console.log(`Igualdad (a == x): ${a == x}`); // true

// Igualdad estricta (===) sin coerción de tipo
console.log(`Igualdad estricta (a === x): ${a === x}`); // false
```
![[Pasted image 20241014111240.png]]
