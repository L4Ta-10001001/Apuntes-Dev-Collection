Este primer contenedor va a tener el protocolo SSH habilitado, donde dentro habrá un servidor de Apache habilitado que indicará un usuario al cual hacer fuerza bruta. Una vez dentro del contenedor, podremos escalar privilegios usando el comando sudo -l del binario vim.

Para configurar el entorno, podemos utilizar o bien la construcción de una imagen a partir de un archivo Dockerfile / Docker-compose o bien entrando dentro del contenedor y de forma manual realizar toda la configuración.

-----

Entramos dentro del contenedor y hacemos toda la configuración:
```bash
apt install openssh-server
apt install nano
adduser mario
```
Al añadir este usuario, le ponemos la contraseña chocolate:
```bash
mario:chocolate
```
![[Pasted image 20240320103804.png]]
Ahora que ya tenemos el protocolo SSH habilitado, vamos a configurar apache:
```bash
apt install apache2
```
Y ahora tenemos el directorio /var/www/html creado:
![[Pasted image 20240320104112.png]]
Vamos a crear un archivo llamado secret.php donde se le de al usuario la pista de la existencia del usuario mario para hacerle fuerza bruta por SSH:
```bash
apt install php
```
![[Pasted image 20240320104318.png]]
El archivo de php tendrá el siguiente contenido:
```php
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>¡Secreto!</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            text-align: center;
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            color: #333;
        }
        p {
            color: #666;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Hola Mario,</h1>
        <p>Esta web no se puede hackear.</p>
    </div>
</body>
</html>
```
Y así es como se ve desde el navegador:
![[Pasted image 20240320104415.png]]
Ahora ya tendremos abiertos tanto el puerto 80 como 22 del contenedor:
![[Pasted image 20240320105636.png]]
Ahora si usamos Hydra, podremos entrar por fuerza bruta:
```bash
hydra -l mario -P rockyou.txt ssh://172.17.0.1
```
![[Pasted image 20240320105803.png]]
Ahora vamos a configurar la escalada de privilegios, donde primero debemos de instalar el binario de vim y después modificar el archivo visudo permitiendo ejecutar como root este binario:
```bash
apt install vim
apt install sudo
```
Y ahora ya podremos abrir el archivo sudoers ejecutando el comando sudo visudo, donde al final del todo añadiremos la siguiente línea:
```bash
mario ALL=(ALL) /usr/bin/vim
```
![[Pasted image 20240320110106.png]]
De esta forma, si ahora hacemos un su mario y ejecutamos el comando sudo -l, vemos que como root este usuario puede ejecutar el siguiente binario:
![[Pasted image 20240320110144.png]]
Donde deberá de insertar el siguiente comando para escalar privilegios:
```bash
sudo -u root /usr/bin/vim -c ':!/bin/sh'
```
![[Pasted image 20240320110252.png]]
Esta máquina ya está lista, por lo que vamos a convertirla en una imagen y subirla a dockerhub:
```bash
docker commit 4ae4083f941a laboratorio1
```
Ya la tenemos en imagen:
![[Pasted image 20240320110447.png]]
Ahora la subimos a dockerhub:
```bash
docker login
```
Iniciamos sesión:
![[Pasted image 20240320110540.png]]
Y etiquetamos nuestra imagen:
```bash
docker tag laboratorio1:latest maalfer/laboratorio1:latest
docker push maalfer/laboratorio1:latest
```
Y ya se sube nuestra imagen:
![[Pasted image 20240320110957.png]]
Lo comprobamos desde dockerhub:
![[Pasted image 20240320111015.png]]
