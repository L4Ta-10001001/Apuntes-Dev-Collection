Tendremos el siguiente Dockerfile:
```bash
FROM ubuntu:latest

# Instalar Apache, mod_wsgi y Python
RUN apt-get update && \
    apt-get install -y python3 python3-pip iputils-ping

# Instalar Flask
RUN apt install -y python3-flask

RUN apt-get install -y xdg-user-dirs

# Crear el usuario freddy y los directorios estándar
RUN useradd -m -s /bin/bash freddy \
    && echo "freddy:JIFGHDS87GYDFIGD" | chpasswd \
    && su - freddy -c "xdg-user-dirs-update"

# Copiar los archivos de la aplicación al contenedor
COPY ./flaskapp /home/freddy/Desktop/flaskapp

# Cambiar el propietario de los archivos de la aplicación
RUN chown -R freddy:freddy /home/freddy/Desktop/flaskapp

RUN chmod u+s /bin/ping

# Definir el comando de inicio del contenedor
CMD su - freddy -c "python3 /home/freddy/Desktop/flaskapp/app.py" ; tail -f /dev/null
```
Y luego tendremos el directorio flaskapp donde se contendrá toda nuestra aplicación:
![[Pasted image 20240519154942.png]]
Este será el contenido de app.py:
```python
from flask import Flask, request, render_template
import subprocess

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def index():
    ip_address = ''
    ping_result = ''
    if request.method == 'POST':
        ip_address = request.form.get('ip_address')
        ping_result = ping_ip(ip_address)
    return render_template('index.html', ip_address=ip_address, ping_result=ping_result)

def ping_ip(ip):
    try:
        cmd = f"ping -c 4 {ip}"
        output = subprocess.check_output(cmd, shell=True, universal_newlines=True)
        return output
    except subprocess.CalledProcessError as e:
        return str(e)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```
Y dentro de templates, tendremos el siguiente index.html:
![[Pasted image 20240519155055.png]]
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ping Test</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #2c3e50;
            color: #ecf0f1;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            background-color: #34495e;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            width: 400px;
            text-align: center;
        }
        input[type="text"] {
            width: calc(100% - 22px);
            padding: 10px;
            margin: 10px 0;
            border: none;
            border-radius: 5px;
        }
        button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            background-color: #1abc9c;
            color: white;
            cursor: pointer;
        }
        button:hover {
            background-color: #16a085;
        }
        .output {
            background-color: #ecf0f1;
            color: #2c3e50;
            padding: 10px;
            border-radius: 5px;
            margin-top: 10px;
            text-align: left;
            height: 150px;
            overflow-y: auto;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Ping Test</h1>
        <form method="post">
            <input type="text" name="ip_address" placeholder="IP Address" value="{{ ip_address }}">
            <button type="submit">Ping!</button>
        </form>
        {% if ping_result %}
            <div class="output">
                <pre>{{ ping_result }}</pre>
            </div>
        {% endif %}
    </div>
</body>
</html>
```
Habremos obtenido lo siguiente:
![[Pasted image 20240519155132.png]]
