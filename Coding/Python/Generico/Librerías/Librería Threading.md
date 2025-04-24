Esta librería de Python permite poder ejecutar una misma tarea de forma simultánea, lo que nos permitirá agilizar el funcionamiento de nuestros script:
```python
import threading

# Función que será ejecutada por cada hilo
def mi_funcion():
    for i in range(5):
        print(f"Hilo actual: {threading.current_thread().name}, Valor: {i}")

# Crear los hilos
thread1 = threading.Thread(target=mi_funcion)
thread2 = threading.Thread(target=mi_funcion)
thread3 = threading.Thread(target=mi_funcion)
thread4 = threading.Thread(target=mi_funcion)
thread5 = threading.Thread(target=mi_funcion)

# Iniciar los hilos
thread1.start()
thread2.start()
thread3.start()
thread4.start()
thread5.start()

# Esperar a que los hilos terminen
thread1.join()
thread2.join()
thread3.join()
thread4.join()
thread5.join()

print("Hilos finalizados.")
```
Y este sería el resultado, donde podremos ver como la función se fue ejecutando de forma simultánea:
![[Pasted image 20240531165027.png]]


