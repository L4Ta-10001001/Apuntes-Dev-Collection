Los objetos en JavaScript son una forma de almacenar colecciones de datos y más complejos entidades. Son estructuras de datos que permiten agrupar propiedades y métodos. Por ejemplo nos creamos el objeto persona y luego podemos acceder a cada una de sus propiedades:
```javascript
const persona = {
    nombre: "Mario",
    edad: 30,
    ciudad: "Madrid"
};

console.log(persona.nombre); // Usando notación de punto
console.log(persona['edad']); // Usando notación de corchetes
```
![[Pasted image 20241008183107.png]]

También podemos cambiar o añadir propiedades:
```javascript
const persona = {
    nombre: "Mario",
    edad: 30,
    ciudad: "Madrid"
};

// Datos antiguos
console.log("El nombre es:", persona.nombre); 
console.log("La edad es:", persona['edad']); 

// Aplicamos cambios
persona.ocupacion = "Desarrollador"; 
persona.edad = 31; 

// Imprimimos las novedades
console.log("Ahora tiene ", persona['edad']); 
console.log("Y ahora es ", persona['ocupacion']); 
```
![[Pasted image 20241008184035.png]]
## MÉTODOS EN OBJETOS
Los objetos también pueden tener métodos, que son funciones asociadas a ese objeto:
```javascript
const coche = {
    marca: "Toyota",
    modelo: "Corolla",
    año: 2021,
    mostrarDetalles: function() {
        return `${coche.marca} ${coche.modelo} (${coche.año})`;
    }
};

console.log(coche.mostrarDetalles()); // Toyota Corolla (2021)
```
![[Pasted image 20241008184611.png]]
**Uso de This**
Dentro de un método de objeto, `this` hace referencia al objeto que está siendo utilizado:
![[Pasted image 20241008184728.png]]
```javascript
const libro = {
    titulo: "JavaScript para Todos",
    autor: "Mario Álvarez",
    mostrarInformacion: function() {
        console.log(`Título: ${this.titulo}, Autor: ${this.autor}`);
    }
};

libro.mostrarInformacion(); // Título: JavaScript para Todos, Autor: Mario Álvarez
```
