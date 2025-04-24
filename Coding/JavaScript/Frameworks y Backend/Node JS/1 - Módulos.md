En Node.js, los módulos son una forma de organizar y reutilizar el código. Cada archivo en Node.js es un módulo por defecto, y los módulos pueden exportar funcionalidad para ser utilizada en otros archivos.
### Tipos de Módulos en Node.js

1. **Módulos Internos (Core Modules)**:
    
    - Node.js viene con una serie de módulos internos, como `http`, `fs`, `path`, `os`, entre otros. Estos módulos se cargan directamente sin necesidad de instalar nada adicional.
2. **Módulos de Usuario**:
    
    - Son módulos creados por el desarrollador y pueden ser utilizados en diferentes partes de la aplicación.
    - Se pueden organizar en archivos y directorios según la estructura y necesidades del proyecto.
3. **Módulos de Terceros**:
    
    - Estos módulos son creados por la comunidad y se encuentran disponibles en el registro de npm (Node Package Manager).
    - Se instalan utilizando npm y se importan en tu código.


---------

Vamos a crear un script llamado maths.js que tendrá dos funciones para hacer algunas operaciones al pasarle dos números:
```javascript
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract
};
```
![[Pasted image 20240730143915.png]]
Y ahora otro código que sea app.js que sea lo que pase los dos números a math.js:
```javascript
// app.js
const math = require('./math');

const sum = math.add(5, 3);
const difference = math.subtract(9, 4);

console.log(`Sum: ${sum}`);
console.log(`Difference: ${difference}`);
```
![[Pasted image 20240730144001.png]]
Entonces ahora si ejecutamos el app.js se enviará al módulo math los dos números y se ejecutará el contenido de maths.js:
![[Pasted image 20240730144037.png]]
## EXPORTAR MÓDULOS INDIVIDUALES
También podemos exportar elementos individualmente en lugar de usar `module.exports` con un objeto, donde primero creamos el siguiente archivo llamado utils.js:
```javascript
// utils.js
exports.saludar = function(name) {
    return `Hello, ${name}!`;
};

exports.despedir = function(name) {
    return `Goodbye, ${name}!`;
};
```
Y un app.js que utilice las funciones de utils.js pasándole los nombres:
```javascript
// app.js
const utils = require('./utils');

console.log(utils.saludar('Alice'));
console.log(utils.despedir('Bob'));
```
![[Pasted image 20240730144503.png]]
Y el archivo app.js habrá utilizado utils.js:
![[Pasted image 20240730144437.png]]
## MÓDULOS INTERNOS
Node.js tiene varios módulos internos que puedes usar sin necesidad de instalar nada. Aquí algunos ejemplos:

**Uso del Módulo `http`:**
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello, World!\n');
});

const PORT = 3000;
server.listen(PORT, () => {
    console.log(`Server running at http://localhost:${PORT}/`);
});
```
**Uso del Módulo `fs`:**
```javascript
const fs = require('fs');

// Leer un archivo
fs.readFile('example.txt', 'utf8', (err, data) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log(data);
});

// Escribir en un archivo
fs.writeFile('example.txt', 'Hello, Node.js!', (err) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log('File written successfully!');
});
```
