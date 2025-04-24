## COMBINAR PYTHON CON HTML
Podemos incorporar HTML con python, de tal forma que podremos establecer un comportamiento de nuestra web con python. Por ejemplo de la siguiente manera, donde ponemos una variable dentro del HTML:
```python
from flask import Flask
import datetime

app = Flask(__name__)

variable = 'mario'

@app.route('/')
def index():
    # Generar el contenido HTML dinámicamente
    content = (f"<h1>Hola, {variable}!</h1>")
    content += "<p>La hora actual es: " + str(datetime.datetime.now()) + "</p>"

    # Devolver el contenido HTML generado como respuesta
    return content

if __name__ == '__main__':
    app.run()
```
Y si ejecutamos este código, por el puerto 5000 tendremos corriendo la web, donde vemos que se está imprimiendo el contenido de la variable que hemos declarado al principio:
![[Pasted image 20230501205759.png]]
### CREAR UN FORMULARIO
Si queremos crear un formulario donde podamos escribir y que se imprima el nombre, lo haríamos así, con el código de app.py:
```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def home():
    nombre = None
    if request.method == 'POST':
        nombre = request.form['nombre']
    return render_template('index.html', nombre=nombre)

if __name__ == '__main__':
    app.run(debug=True)
```
Y con este HTML:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulario con Flask</title>
</head>
<body>
    <h1>Formulario con Flask</h1>

    {% if nombre %}
        <h2>¡Hola, {{ nombre }}!</h2>
    {% else %}
        <h2>¡Hola, desconocido!</h2>
    {% endif %}

    <form method="POST">
        <label for="nombre">Ingresa tu nombre:</label>
        <input type="text" id="nombre" name="nombre" required>
        <button type="submit">Enviar</button>
    </form>
</body>
</html>
```
![[Pasted image 20240523230738.png]]
