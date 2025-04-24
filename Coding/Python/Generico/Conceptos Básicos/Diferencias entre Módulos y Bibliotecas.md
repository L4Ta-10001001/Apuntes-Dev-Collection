1. **Ejemplo de Módulo:**

Supongamos que tienes un archivo llamado `operaciones.py` que contiene algunas funciones matemáticas básicas:
```python
# operaciones.py
def suma(a, b):
    return a + b

def resta(a, b):
    return a - b

def multiplicacion(a, b):
    return a * b
```
Este archivo `operaciones.py` es un módulo que contiene definiciones de funciones. Puedes importar este módulo en otro script y usar sus funciones, como este:
```python
# main.py
import operaciones

resultado_suma = operaciones.suma(5, 3)
resultado_resta = operaciones.resta(8, 2)

print("Resultado de la suma:", resultado_suma)
print("Resultado de la resta:", resultado_resta)
```
En este ejemplo, `operaciones.py` es un módulo que contiene funciones matemáticas, y `main.py` es otro script que importa este módulo y usa sus funciones.

2. **Ejemplo de Biblioteca:**

Digamos que quieres trabajar con fechas y horarios en Python. Puedes utilizar la biblioteca estándar `datetime`, que proporciona funcionalidades para manejar fechas y horarios de una manera conveniente. Aquí tienes un ejemplo de cómo usar esta biblioteca:
```python
import datetime

fecha_actual = datetime.datetime.now()
print("Fecha y hora actual:", fecha_actual)

anio_actual = fecha_actual.year
print("Año actual:", anio_actual)
```
En este ejemplo, `datetime` es una biblioteca que contiene múltiples módulos, incluido `datetime`, que proporciona clases y funciones para trabajar con fechas y horas. Estamos importando el módulo `datetime` de la biblioteca `datetime` para obtener la fecha y hora actual, y luego accedemos a sus funciones y clases para obtener información específica sobre la fecha y la hora actual.

En resumen, el primer ejemplo muestra un módulo (`operaciones.py`) que contiene funciones relacionadas, mientras que el segundo ejemplo muestra una biblioteca (`datetime`) que proporciona funcionalidades más amplias relacionadas con el manejo de fechas y horas en Python.