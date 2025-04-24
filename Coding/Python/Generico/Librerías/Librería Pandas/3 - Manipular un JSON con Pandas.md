Tenemos el siguiente archivo en formato JSON:
```python
{
  "libros": [
    {
      "titulo": "Python for Data Science",
      "autor": "John Smith",
      "año_publicacion": 2019,
      "precio": 29.99
    },
    {
      "titulo": "Machine Learning Basics",
      "autor": "Alice Johnson",
      "año_publicacion": 2020,
      "precio": 45.99
    },
    {
      "titulo": "Data Visualization with Matplotlib",
      "autor": "David Williams",
      "año_publicacion": 2018,
      "precio": 24.99
    }
  ]
}
```
Y con el siguiente código de pandas podremos convertir este JSON en un DataFrame:
```python
import pandas as pd
import json

# Ruta al archivo JSON
archivo_json = 'datos.json'

# Leer el JSON desde el archivo
with open(archivo_json, 'r') as file:
    datos_json = json.load(file)

# Crear un DataFrame desde el JSON
df = pd.json_normalize(datos_json, 'libros')

# Mostrar el DataFrame
print(df)
```
![[Pasted image 20240107194759.png]]
