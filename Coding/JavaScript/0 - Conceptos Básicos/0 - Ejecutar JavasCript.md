# Ejecutar JavasCript en HTML

Para ejecutar código javascript dentro de un HTML, lo hacemos dentro de la etiqueta script:
```javascript
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ejemplo de JavaScript en HTML</title>
</head>
<body>
    <h1>Hola Mundo</h1>
    <p id="demo">Este es un ejemplo de JavaScript en HTML.</p>

    <script>
        // Código JavaScript sencillo con espera de 3 segundos
        document.addEventListener('DOMContentLoaded', function() {
            // Espera de 3 segundos (3000 milisegundos) antes de cambiar el contenido del párrafo
            setTimeout(function() {
                document.getElementById("demo").innerHTML = "¡El JavaScript ha cambiado este texto después de 3 segundos!";
            }, 3000);
        });
    </script>
</body>
</html>
```
Donde primero se ejecuta el mensaje de HTML:
![[Pasted image 20240705094058.png]]
Y luego se actualiza con el de javascript:
![[Pasted image 20240705094114.png]]
### MÉTODO getElementById
`document.getElementById()` es un método en JavaScript que se utiliza para acceder a un elemento HTML en el documento actual a través de su atributo `id`.

Cuando se llama a `document.getElementById('miId')`, el navegador busca en el DOM del documento HTML el primer elemento que tenga un atributo `id` con el valor `'miId'`. Si encuentra ese elemento, lo devuelve como un objeto `Element`, lo que permite manipularlo mediante JavaScript.

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Imprimir texto</title>
</head>
<body>
    <!-- Campo de entrada para el usuario -->
    <input type="text" id="userInput" placeholder="Escribe algo aquí">
    
    <!-- Botón que al presionarse ejecuta una función -->
    <button onclick="printText()">Imprimir texto</button>
    
    <!-- Lugar donde se mostrará el texto -->
    <p id="output"></p>

    <!-- Script de JavaScript -->
    <script>
        // Función que se ejecuta al presionar el botón
        function printText() {
            // Obtener el valor del campo de entrada
            let inputText = document.getElementById('userInput').value;
            
            // Mostrar el texto en el párrafo con id 'output'
            document.getElementById('output').innerText = inputText;
        }
    </script>
</body>
</html>
```
Y ahora para poder obtener el valor del campo de entrada, se usa getElementById y así poder modificarlo con javascript:
```javascript
let inputText = document.getElementById('userInput').value;
```
![[Pasted image 20240705100734.png]]
En resumen, `document.getElementById()` es una herramienta poderosa y eficiente para seleccionar y manipular elementos HTML individuales basándose en su atributo `id`.
# Ejecutar JavasCript en Terminal

Node.js es un entorno de ejecución de JavaScript que permite ejecutar código JavaScript fuera de un navegador web, es decir, en el entorno del servidor o en la línea de comandos del sistema, y podemos usar NPM para gestionar las dependencias:
```javascript
console.log("Hola mundo")
```
Y ahora si lo queremos ejecutar, podemos hacerlo desde el terminal:
![[Pasted image 20240705102437.png]]
