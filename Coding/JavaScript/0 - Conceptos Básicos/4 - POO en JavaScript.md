```javascript
// Clase persona usando una función constructora
function Persona(nombre, edad) {
    this.nombre = nombre;
    this.edad = edad;
    
    // Método
    this.imprimirDetalles = function() {
        console.log(`Nombre: ${this.nombre}, Edad: ${this.edad}`);
    };
}

// Creación de instancias (objetos) de la clase Persona
let persona1 = new Persona('Juan', 30);
let persona2 = new Persona('María', 25);

persona1.imprimirDetalles();  // Imprime "Nombre: Juan, Edad: 30"
persona2.imprimirDetalles();  // Imprime "Nombre: María, Edad: 25"
```
Ejecutaríamos lo siguiente:
![[Pasted image 20240705104548.png]]
