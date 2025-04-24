## VENTANAS EMERGENTES
### 1. Usando Ventanas de Diálogo

Tkinter tiene módulos de diálogo predefinidos, como `messagebox` para mostrar mensajes simples y `simpledialog` para pedir entradas del usuario.

**Ejemplo con messagebox**
El módulo `messagebox` se utiliza para mostrar cuadros de diálogo informativos al usuario. Estos cuadros de diálogo pueden contener mensajes de información, advertencias, errores, preguntas de confirmación, entre otros. `messagebox` es principalmente para mostrar mensajes y obtener una respuesta simple (sí/no, aceptar/cancelar).
```PYTHON
import tkinter as tk
from tkinter import messagebox

def popup():
    messagebox.showinfo("Información", "Este es un mensaje de información.")

ventana = tk.Tk()
ventana.title("Ventana Principal")

info_button = tk.Button(ventana, text="Mostrar Información", command=popup)
info_button.pack(pady=20)

ventana.mainloop()
```
![[Pasted image 20240528103946.png]]
**Ejemplo con simpledialog**
El módulo `simpledialog` se utiliza para mostrar cuadros de diálogo que solicitan la entrada del usuario. Es útil cuando necesitas obtener datos específicos del usuario, como un número, una cadena de texto, etc
```python
import tkinter as tk
from tkinter import simpledialog

def ask_name():
    name = simpledialog.askstring("Input", "¿Cuál es tu nombre?")
    if name:
        label.config(text=f"Hola, {name}!")

root = tk.Tk()
root.title("Ventana Principal")

label = tk.Label(root, text="Presiona el botón para ingresar tu nombre.")
label.pack(pady=20)

name_button = tk.Button(root, text="Ingresar Nombre", command=ask_name)
name_button.pack(pady=20)

root.mainloop()
```
![[Pasted image 20240528104127.png]]
## MENÚS DE DESPLIEGUE
```python
import tkinter as tk
from tkinter import messagebox

def nuevo_archivo():
    messagebox.showinfo("Nuevo Archivo", "Crear un nuevo archivo")

def abrir_archivo():
    messagebox.showinfo("Abrir Archivo", "Abrir un archivo existente")

def guardar_archivo():
    messagebox.showinfo("Guardar Archivo", "Guardar el archivo actual")

def salir():
    ventana.quit()

def mostrar_ayuda():
    messagebox.showinfo("Ayuda", "Esta es la sección de ayuda")

def acerca_de():
    messagebox.showinfo("Acerca de", "Esta aplicación fue creada como ejemplo")

# Crear la ventana principal
ventana = tk.Tk()
ventana.title("Ventana con Menú")

# Crear la barra de menús
barra_menus = tk.Menu(ventana)

# Crear el menú Archivo
menu_archivo = tk.Menu(barra_menus, tearoff=0)
menu_archivo.add_command(label="Nuevo", command=nuevo_archivo)
menu_archivo.add_command(label="Abrir", command=abrir_archivo)
menu_archivo.add_command(label="Guardar", command=guardar_archivo)
menu_archivo.add_separator()
menu_archivo.add_command(label="Salir", command=salir)
barra_menus.add_cascade(label="Archivo", menu=menu_archivo)

# Crear el menú Ayuda
menu_ayuda = tk.Menu(barra_menus, tearoff=0)
menu_ayuda.add_command(label="Ayuda", command=mostrar_ayuda)
menu_ayuda.add_command(label="Acerca de", command=acerca_de)
barra_menus.add_cascade(label="Ayuda", menu=menu_ayuda)

# Configurar la barra de menús en la ventana
ventana.config(menu=barra_menus)

# Iniciar el bucle principal de la aplicación
ventana.mainloop()
```
![[Pasted image 20240528104456.png]]
