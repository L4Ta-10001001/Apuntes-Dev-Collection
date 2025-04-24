Paramiko es una librería que se utiliza para la automatización de tareas que requieren conectividad a través del protocolo SSH:
```python
import paramiko
import time

host = '192.168.1.9' # Ponemos los datos del servidor donde nos queramos conectar.
user = 'mario'
password = '123123'

try:

    client = paramiko.SSHClient()
    client.load_host_keys('C:/Users/mario/.ssh/known_hosts') # Aquí cargamos este fichero para indicar que el host ya es conocido.
    client.connect(host, username=user, password=password) # Cargamos las variables.
    client.set_missing_host_key_policy( paramiko.AutoAddPolicy() ) # Esto es para que el host sea conocido por la máquina.

    stdin, stdout, stderr = client.exec_command('ls') # En estas 3 variables se guardan los posibles errores o salidas normales del comando.
    time.sleep(2) # Damos tiempo a que los parámetros se hayan recogido en el punto anterior.
    resultado = stdout.read().decode() # Recogemos el standar output en una variable.
    print(resultado)

except:
    print("Algo ha fallado en la conexión...")
```
![[Pasted image 20221217085839.png]]
## EJECUTAR COMANDOS POR SSH CON PYTHON
También podemos ejecutar el comando para montar un servidor web con python y así poder robar los archivos:
```python
import paramiko

host = '192.168.0.9' # Ponemos los datos del servidor donde nos queramos conectar.
user = 'mario'
password = '123123'

try:

    client = paramiko.SSHClient()
    client.load_host_keys('C:/Users/mario/.ssh/known_hosts')
    client.connect(host, username=user, password=password)
    client.set_missing_host_key_policy( paramiko.AutoAddPolicy() )

    print("Debes escribir esta dirección en el navegador para acceder a los archivos de la víctima: ",host)

    client.exec_command('python3 -m http.server 81') # Montramos el servidor Python por el puerto que queramos.

except:
    print("Hubo un error...")
```
Y ya tenemos acceso desde nuestra máquina atacante a los archivos de la víctima:
![[Pasted image 20221217085936.png]]
## TRANSFERENCIA DE ARCHIVOS POR SCP
**Subir un archivo**
```python
import paramiko
from scp import SCPClient, SCPException

# Datos de conexión
ip = '192.168.0.25'
username = 'pinguino'
password = 'pinguinasio'
local_file = 'nota.txt'
remote_file = 'nota.txt'

try:
    # Crear un cliente SSH
    client = paramiko.SSHClient()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    
    # Conectarse al servidor
    client.connect(ip, username=username, password=password)
    
    # Crear un cliente SCP
    with SCPClient(client.get_transport()) as scp:
        scp.put(local_file, remote_file)
    print("Archivo subido exitosamente.")

except:
    print(f"Error estableciendo conexión SSH")
finally:
    # Cerrar la conexión
    client.close()
```
**Descargar un archivo**
```python
import paramiko
from scp import SCPClient, SCPException

# Datos de conexión
ip = '192.168.0.25'
username = 'pinguino'
password = 'pinguinasio'
local_file = 'nota.txt'
remote_file = 'nota.txt'

try:
    # Crear un cliente SSH
    client = paramiko.SSHClient()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    
    # Conectarse al servidor
    client.connect(ip, username=username, password=password)
    
    # Crear un cliente SCP
    with SCPClient(client.get_transport()) as scp:
        # Descargar el archivo
        scp.get(remote_file, local_file)
    print("Archivo descargado exitosamente.")

except:
    print("Error de autenticación, por favor verifica tus credenciales.")
finally:
    # Cerrar la conexión
    client.close()
```
## AUTOMATIZAR COPIAS DE SEGURIDAD + SUBIDA DE ARCHIVOS SSH
```python
import paramiko
from scp import SCPClient, SCPException
import zipfile
import os

# Datos de conexión
ip = '192.168.0.25'
username = 'pinguino'
password = 'pinguinasio'

try:
    # Crear un cliente SSH
    client = paramiko.SSHClient()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    
    # Conectarse al servidor
    client.connect(ip, username=username, password=password)

    zip_filename = 'scripts.zip'
    extensiones_scripts = ('.sh', '.py')

    with zipfile.ZipFile(zip_filename, 'w') as zipf:
        for archivo in os.listdir():
            if archivo.endswith(extensiones_scripts):
                zipf.write(archivo)
    
    # Crear un cliente SCP
    with SCPClient(client.get_transport()) as scp:
        scp.put(zip_filename, zip_filename)
        os.remove(zip_filename)
    print("Archivo subido exitosamente.")

except:
    print("Error de autenticación, por favor verifica tus credenciales.")
finally:
    # Cerrar la conexión
    client.close()
```
