Primero creamos la estructura en HTML y después con javascript modificamos el input y los botones:
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
![[Pasted image 20240705100213.png]]
## Creación de un popup
Podemos crear un popup al momento de hacer clic a un botón:
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
    <button onclick="showPopup()">Mostrar Popup</button>
    
    <script>
        // Función que se ejecuta al presionar el botón
        function showPopup() {
            // Obtener el valor del campo de entrada
            let inputText = document.getElementById('userInput').value;
            
            // Mostrar el texto en un popup
            alert(inputText);
        }
    </script>
</body>
</html>
```
![[Pasted image 20240730145754.png]]
También podemos hacer un popup personalizado, de tal forma que utilizaremos HTML, CSS y Javascript:
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Imprimir texto</title>
    <style>
        /* Estilos para el popup */
        .popup {
            display: none; /* Oculto por defecto */
            position: fixed;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            background-color: black;
            color: white;
            border: 2px solid white;
            padding: 20px;
            z-index: 1000;
        }

        /* Estilos para el overlay (fondo oscurecido) */
        .overlay {
            display: none; /* Oculto por defecto */
            position: fixed;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 999;
        }

        /* Botón para cerrar el popup */
        .close-btn {
            background-color: white;
            color: black;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <!-- Campo de entrada para el usuario -->
    <input type="text" id="userInput" placeholder="Escribe algo aquí">
    
    <!-- Botón que al presionarse ejecuta una función -->
    <button onclick="showPopup()">Mostrar Popup</button>
    
    <!-- Overlay para el popup -->
    <div id="overlay" class="overlay"></div>

    <!-- Popup personalizado -->
    <div id="popup" class="popup">
        <p id="popupText"></p>
        <button class="close-btn" onclick="closePopup()">Cerrar</button>
    </div>

    <script>
        // Función que se ejecuta al presionar el botón
        function showPopup() {
            // Obtener el valor del campo de entrada
            let inputText = document.getElementById('userInput').value;
            
            // Mostrar el texto en el popup
            document.getElementById('popupText').innerText = inputText;
            
            // Mostrar el popup y el overlay
            document.getElementById('popup').style.display = 'block';
            document.getElementById('overlay').style.display = 'block';
        }

        // Función para cerrar el popup
        function closePopup() {
            // Ocultar el popup y el overlay
            document.getElementById('popup').style.display = 'none';
            document.getElementById('overlay').style.display = 'none';
        }
    </script>
</body>
</html>
```
![[Pasted image 20240730150131.png]]
