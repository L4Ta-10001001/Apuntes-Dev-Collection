Un condicional if en JavaScript permite ejecutar un bloque de código si una condición específica se evalúa como true:
```javascript
let calificacion = 76;

if (calificacion >= 90) {
    console.log("Excelente");

} else if (calificacion >= 80) {
    console.log("Muy Bien");

} else if (calificacion >= 70) {
    console.log("Bien");

} else if (calificacion >= 60) {
    console.log("Suficiente");
    
} else {
    console.log("Insuficiente");
}
```
![[Pasted image 20241014115619.png]]
También podemos usar condicionales negativos con la exclamación `!` 
```javascript
let quieroCebolla = false;

if (quieroCebolla != true){
    console.log("Tu comida no lleva cebolla");

}else{
    console.log("Tu comida sí lleva cebolla");
}
```
![[Pasted image 20241014120044.png]]
