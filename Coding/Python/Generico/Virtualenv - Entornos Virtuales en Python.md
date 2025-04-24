Un entorno virtual en Python consiste en abstraer y aislar configuraciones de Python y sus librerías, usándolas sólamente para un determinado proyecto; y así no lo mezclamos con nuestra configuración de Python de nuestra máquina host. Por ejemplo yo tengo estas librerías de Python configuradas de forma global:
![[Pasted image 20230123153952.png]]
Ahora voy a crear un entorno virtual con la librería virtualenv donde estén las librerías virtualizas que utilizamos para un determinado proyecto; por tanto vamos a hacer un pip install virtualenv:
![[Pasted image 20230123154056.png]]
Ahora vamos a crear el entorno virtual de esta forma, donde le llamaré nombre_entorno:
![[Pasted image 20230123154213.png]]
Y se nos habrán creado estas carpetas dentro del actual directorio de trabajo:
![[Pasted image 20230123154300.png]]
Ahora debemos activar este entorno virtual; y para ello debemos acceder al fichero activate, dentro de la carpeta bin, por tanto usaremos este comando; y veremos que ya estamos dentro del entorno virtual  (en caso de usar Linux9):
![[Pasted image 20230123155658.png]]
Y aquí veremos que ya tenemos otras librerías aisladas de nuestra máquina:
![[Pasted image 20230123155739.png]]
Y aquí ya podemos ir ejecutando nuestro código de Python desde la parte del terminal en Visual Studio Code:
![[Pasted image 20230123160037.png]]
# OPCIÓN 2
Para crear un entorno virtual en Python y poder instalar librerías de forma aislada del resto del sistema, puedes seguir estos pasos:
```bash
python -m venv nombre_del_entorno
```
Y después lo activamos de esta forma:
```bash
source nombre_del_entorno/bin/activate
```
![[Pasted image 20240922094847.png]]
Ahora dentro de este entorno podremos instalar librerías de forma aislada:
```bash
pip install nombre_de_la_libreria
```
Cuando termines de trabajar en el entorno, puedes desactivarlo con:
```bash
deactivate
```
De tal forma que veremos una carpeta que es donde se encuentra el entorno aislado:
![[Pasted image 20240922094959.png]]
Pudiendo volver a entrar en ella con el mismo comando para activarla:
![[Pasted image 20240922095023.png]]
