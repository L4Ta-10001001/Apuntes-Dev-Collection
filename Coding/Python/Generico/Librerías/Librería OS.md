  
El módulo `os` en Python proporciona una interfaz para trabajar con el sistema operativo, permitiendo realizar diversas operaciones relacionadas con archivos, directorios, procesos, variables de entorno, y más. Sin embargo, debido a la cantidad de contenido que tiene el módulo `os`, no es práctico mostrar todo su contenido aquí. Pero puedo darte una visión general de algunas de las funciones y constantes más comunes disponibles en el módulo `os`.
## OBTENER RUTA ABSOLUTA DE UN ARCHIVO
```python
import os

print(os.path.abspath('pruebas.txt'))
```
Y este sería el resultado:
![[Pasted image 20230524150643.png]]
## OBTENER LA EXTENSIÓN DE UN ARCHIVO
Podemos filtrar por un determinado patrón en un archivo, por ejemplo para obtener la extensión de un archivo:
```python
import os

nombre_base = os.path.splitext("video.avi")[1]
print(nombre_base)
```
![[Pasted image 20230928135240.png]]
## OTROS EJEMPLOS
```python
import os

# Funciones comunes
print("Directorio actual:", os.getcwd())  # Obtener el directorio de trabajo actual
print("Listado de archivos en el directorio actual:", os.listdir())  # Listar archivos en el directorio actual
print("Separador de ruta del sistema:", os.sep)  # Separador de ruta del sistema
print("Variables de entorno:", os.environ)  # Variables de entorno del sistema

# Constantes útiles
print("Separador de ruta del sistema:", os.sep)  # Separador de ruta del sistema
print("Variable de entorno para el directorio de inicio del usuario:", os.path.expanduser('~'))  # Directorio de inicio del usuario
print("Directorio de inicio del usuario:", os.path.expanduser('~'))  # Directorio de inicio del usuario
```
El módulo `os` contiene muchas más funciones y constantes útiles que puedes explorar en la documentación oficial de Python o mediante la función `help(os)` en un entorno interactivo de Python.