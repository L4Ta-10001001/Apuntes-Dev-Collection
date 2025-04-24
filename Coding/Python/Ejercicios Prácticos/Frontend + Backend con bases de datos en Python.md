```python
import tkinter as tk
from tkinter import messagebox
import mysql.connector

def insertar_datos(usuario, contraseña):
    try:
        conexion = mysql.connector.connect(
            host='localhost',
            database='pruebas',
            user='mario',
            password='123123',
            auth_plugin='mysql_native_password'
        )
    
        if conexion.is_connected():
            cursor = conexion.cursor()
            cursor.execute(f'''
            INSERT INTO usuarios (nombre, contraseña) VALUES ('{usuario}', '{contraseña}')
            ''')

            conexion.commit()
            cursor.close()
            conexion.close()
            return True

    except mysql.connector.Error as error:
        print("Hubo un problema al conectarse a la base de datos: ", error)
        return False

def registrar_usuario():
    usuario = entry_usuario.get()
    contraseña = entry_contraseña.get()
    
    if insertar_datos(usuario, contraseña):
        messagebox.showinfo("Éxito", "Usuario registrado correctamente")
    else:
        messagebox.showerror("Error", "No se pudo registrar el usuario")

# Configuración de la ventana principal
ventana = tk.Tk()
ventana.title("Registro de Usuario")

# Diseño de la interfaz
frame = tk.Frame(ventana)
frame.pack(padx=20, pady=20)

tk.Label(frame, text="Usuario").pack(pady=5)
entry_usuario = tk.Entry(frame)
entry_usuario.pack(pady=5)

tk.Label(frame, text="Contraseña").pack(pady=5)
entry_contraseña = tk.Entry(frame, show='*')
entry_contraseña.pack(pady=5)

tk.Button(frame, text="Registrar", command=registrar_usuario).pack(pady=20)

ventana.mainloop()
```
![[Pasted image 20240527140902.png]]
