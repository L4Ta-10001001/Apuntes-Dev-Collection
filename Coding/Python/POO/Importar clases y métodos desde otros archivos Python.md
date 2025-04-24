Supongamos que tenemos dos archivos: `clase.py` y `main.py`.

En `clase.py`, definimos una clase llamada `MiClase`:
```python
# clase.py

class MiClase:
    def __init__(self, nombre):
        self.nombre = nombre

    def saludar(self):
        print(f"Hola, soy {self.nombre}")
```
En `main.py`, importamos la clase `MiClase` desde `clase.py` y la utilizamos:
```python
# main.py

from clase import MiClase

# Creamos una instancia de MiClase
objeto = MiClase("ChatGPT")

# Llamamos al método saludar de la instancia
objeto.saludar()
```
Al ejecutar `main.py`, obtendrás la salida:
```python
Hola, soy ChatGPT
```
