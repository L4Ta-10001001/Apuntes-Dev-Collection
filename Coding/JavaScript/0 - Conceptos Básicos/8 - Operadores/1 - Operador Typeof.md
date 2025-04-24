El operador `typeof` se utiliza para determinar el tipo de una variable o un valor. Devuelve una cadena que representa el tipo del operando.
```javascript
let frase = "Hola soy Mario";
let anio = 2027;            
let interes = 2.7;          
let mayorEdad = true;    
let vacia;                   
let nula = null;              

let frutas = ["fresa", "sandia", "naranja"]; 
let hero = { nombre: "Batman", universo: "DC" };

console.log(frase, interes, mayorEdad);

console.log(typeof frase); // Dirá que frase es un string.
```
![[Pasted image 20241014105335.png]]
También podemos comprobar si un dato es un array:
```javascript
let frase = "Hola soy Mario";
let anio = 2027;            
let interes = 2.7;          
let mayorEdad = true;    
let vacia;                   
let nula = null;              

let frutas = ["fresa", "sandia", "naranja"]; 
let hero = { nombre: "Batman", universo: "DC" };

console.log(frase, interes, mayorEdad);

console.log(Array.isArray(frutas)); // Dirá que es true
```
![[Pasted image 20241014105541.png]]
