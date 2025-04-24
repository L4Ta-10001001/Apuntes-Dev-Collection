# DESDE EL TERMINAL
### VARIABLES
- **var** -> Funciona en todo el bloque de la función donde se declara. Si se declara fuera de una función, es global.
```javascript
var x = 10;
if (true) {
    var x = 20; // misma variable
}
console.log(x); // 20
```
- **let** -> Funciona solo dentro del bloque donde se declara:
```javascript
let y = 10;
if (true) {
    let y = 20; // variable diferente dentro del bloque
    console.log(y); // 20
}
console.log(y); // 10
```
- **const** -> Similar a `let`, pero no puedes usar la variable antes de declararla.
```javascript
const z = 10;
if (true) {
    const z = 20; // variable diferente dentro del bloque
    console.log(z); // 20
}
console.log(z); // 10
```
Ejecutamos el siguiente ejemplo de uso:
```javascript
var nombre = 'Juan'; // Ejemplo utilizando var (forma antigua)
let edad = 30;       // Ejemplo utilizando let (forma moderna, preferida)
const PI = 3.1416;   // Ejemplo utilizando const (constante, no puede ser reasignada)

console.log("El nombre es: ",nombre)
console.log("La edad es: ",edad)
console.log("El número PI es: ",PI)
```
![[Pasted image 20240705103039.png]]
### FUNCIONES
```javascript
function saludar(nombre) {
    return 'Hola, ' + nombre + '!';
}

console.log(saludar('Ana')); // Llamada a la función
```
![[Pasted image 20240705103454.png]]
También existen las funciones flecha, las cuales se crean utilizando => 
```javascript
const saludar = () => {
    console.log("¡Hola!");
};

saludar()
```
![[Pasted image 20240705111034.png]]
Pudiendo pasarle como parámetro un nombre a la función:
```javascript
const saludar = (nombre) => {
    console.log("¡Hola!", nombre);
};

saludar("Mario")
```
![[Pasted image 20240705111146.png]]
### ESTRUCTURAS DE CONTROL
```javascript
let nombre = "Mario";

if (nombre = "Mario") {
    console.log('El nombre es Mario');
} else if (nombre = "Juan") {
    console.log('El nombre es Juan');
} else {
    console.log('El nombre no es ninguno de los anteriores');
}
```
![[Pasted image 20240705103657.png]]
# MODIFICANDO EL HTML
### VARIABLES
En el siguiente ejemplo vamos a crear 3 variables diferentes, utilizando var, let y const:
```javascript
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Variables en JavaScript</title>
</head>
<body>
    <h1>Variables en JavaScript</h1>
    <p id="variableDemo"></p>

    <script>
        // Declaración de variables
        var nombre = "Juan";
        let edad = 30;
        const pais = "España";

        // Mostrar variables en el párrafo
        document.getElementById("variableDemo").innerHTML = `Nombre: ${nombre}, Edad: ${edad}, País: ${pais}`;
    </script>
</body>
</html>
```
![[Pasted image 20240705094345.png]]

-----

Ahora para ejecutar estos ejemplos en el navegador, vamos a incrustar este código dentro de un archivo .html:
```javascript
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ejemplos de var, let y const</title>
</head>
<body>
    <h1>Ejemplos de var, let y const</h1>
    <h2>Ejemplo con var</h2>
    <p id="varDemo"></p>
    
    <h2>Ejemplo con let</h2>
    <p id="letDemo"></p>
    
    <h2>Ejemplo con const</h2>
    <p id="constDemo"></p>

    <script>
        // Ejemplo con var
        var x = 10;
        if (true) {
            var x = 20; // misma variable
        }
        document.getElementById("varDemo").innerText = "var x: " + x; // 20

        // Ejemplo con let
        let y = 10;
        if (true) {
            let y = 20; // variable diferente dentro del bloque
            console.log(y); // 20
        }
        console.log(y); // 10
        document.getElementById("letDemo").innerText = "let y: " + y; // 10

        // Ejemplo con const
        const z = 10;
        if (true) {
            const z = 20; // variable diferente dentro del bloque
            console.log(z); // 20
        }
        console.log(z); // 10
        document.getElementById("constDemo").innerText = "const z: " + z; // 10
    </script>
</body>
</html>
```
Y esto es lo que se ejecuta en el navegador:
![[Pasted image 20240705095219.png]]
### OPERADORES

```javascript
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Operadores en JavaScript</title>
</head>
<body>
    <h1>Operadores en JavaScript</h1>
    <p id="operadorDemo"></p>

    <script>
        // Operadores aritméticos
        let a = 10;
        let b = 5;
        let suma = a + b;
        let resta = a - b;
        let multiplicacion = a * b;
        let division = a / b;

        // Mostrar resultados en el párrafo
        document.getElementById("operadorDemo").innerHTML = `Suma: ${suma}, Resta: ${resta}, Multiplicación: ${multiplicacion}, División: ${division}`;
    </script>
</body>
</html>
```
![[Pasted image 20240705095423.png]]
### FUNCIONES

```javascript
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Funciones en JavaScript</title>
</head>
<body>
    <h1>Funciones en JavaScript</h1>
    <p id="funcionDemo"></p>

    <script>
        // Declaración de una función
        function saludar(nombre) {
            return `Hola, ${nombre}!`;
        }

        // Llamar a la función y mostrar el resultado
        document.getElementById("funcionDemo").innerHTML = saludar("María");
    </script>
</body>
</html>
```
![[Pasted image 20240705095617.png]]
### ESTRUCTURAS DE CONTROL
```javascript
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Estructuras de Control en JavaScript</title>
</head>
<body>
    <h1>Estructuras de Control en JavaScript</h1>
    <p id="controlDemo"></p>

    <script>
        // Estructura de control if-else
        let numero = 10;
        let mensaje;

        if (numero > 5) {
            mensaje = "El número es mayor que 5";
        } else {
            mensaje = "El número es 5 o menor";
        }

        // Mostrar el mensaje en el párrafo
        document.getElementById("controlDemo").innerHTML = mensaje;
    </script>
</body>
</html>
```
![[Pasted image 20240705095652.png]]
### MODIFICAR EL DOM
El DOM es la interfaz de programación para los documentos HTML y XML, y de la siguiente manera se puede modificar:
```javascript
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Manipulación del DOM en JavaScript</title>
</head>
<body>
    <h1>Manipulación del DOM en JavaScript</h1>
    <p id="domDemo">Texto original.</p>
    <button onclick="cambiarTexto()">Cambiar Texto</button>

    <script>
        // Función para cambiar el contenido del párrafo
        function cambiarTexto() {
            document.getElementById("domDemo").innerHTML = "¡El texto ha sido cambiado!";
        }
    </script>
</body>
</html>
```
![[Pasted image 20240705095739.png]]
Si hacemos un clic en el botón, veremos que el texto se cambia:
![[Pasted image 20240705095754.png]]
