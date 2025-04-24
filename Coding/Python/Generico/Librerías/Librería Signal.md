La librería signal en Python se utiliza para manejar señales del sistema. Una señal es una notificación asíncrona enviada a un proceso para notificarle de un evento. Las señales son una forma común en sistemas Unix para manejar eventos como interrupciones de teclado (Ctrl+C), temporizadores y otros eventos del sistema.

---------

**SIGINT**
Aquí hay un ejemplo básico de cómo utilizar `signal` para manejar una interrupción de teclado (Ctrl+C):
```python
import signal
import time

def pulsarcontrolc(sig, frame):
    print('¡Se recibió una interrupción de teclado!')
    exit(0)

# Asigna la señal SIGINT (Ctrl+C) a la función pulsarcontrolc
signal.signal(signal.SIGINT, pulsarcontrolc)

print('Presiona Ctrl+C para interrumpir')
while True:
    time.sleep(1)
```
**SIGTERM**
SIGTERM se envía típicamente para solicitar la terminación del programa:
```python
import signal
import time

def detener(sig, frame):
    print('¡Se recibió una señal SIGTERM!')
    exit(0)

# SIGTERM se ejecutará para detener el programa
signal.signal(signal.SIGTERM, detener)

print('Esperando la señal SIGTERM (por ejemplo si ejecutamos "kill -TERM <pid>" desde otro terminal)')
while True:
    time.sleep(1)
```
Por ejemplo, ejecutamos este código, y se quedará esperando:
![[Pasted image 20240531112420.png]]
Y si desde otra terminal, buscamos el proceso, lo encontramos:
```bash
ps -e | grep python
```
![[Pasted image 20240531112442.png]]
Lo detenemos:
![[Pasted image 20240531112451.png]]
Y el script recibe la señal por SIGTERM:
![[Pasted image 20240531112505.png]]
