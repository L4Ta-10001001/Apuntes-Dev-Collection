Esta máquina vulnerable va a consistir en un servidor de apache donde tendremos dos archivos .php y .html, donde uno de ellos se encargará de mostrar al usuario la web donde poder subir un archivo y mientras tanto el otro se encargará de procesar dicho archivo y guardarlo dentro del directorio /uploads.

La idea será que el usuario pueda subir un archivo .php malicioso como una webshell o una reverse shell, de tal forma que pueda acceder a ella en el directorio /uploads (que se deberá encontrar realizando fuzzing web), y una vez encontrado, se podrá utilizar el archivo malicioso subido para obtener ejecución remota de comandos y una intrusión al servidor.

Una vez obtenido acceso al servidor, vamos a configurar un binario vulnerable a escalada de privilegios, el cual utilizaremos /env, para que de esta forma podamos ejecutar comandos como root utilizando dicho binario, y proceder a la escalada de privilegios.

---------------

Vamos a tener los siguientes códigos, uno que será un index.html donde se ofrezca al usuario la posibilidad de subir un archivo:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Upload here your file</title>
    <style>
        body {
            background-color: #222;
            color: #fff;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
        }
        h2 {
            margin-top: 50px;
        }
        form {
            margin-top: 20px;
        }
        input[type="file"] {
            border: 2px solid #444;
            border-radius: 5px;
            background-color: #333;
            color: #fff;
            padding: 10px;
            width: 300px;
            max-width: 100%;
            margin-bottom: 20px;
            box-sizing: border-box;
        }
        input[type="submit"] {
            background-color: #4CAF50;
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        input[type="submit"]:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body> 
    <h2>Upload File</h2>
    <form action="upload.php" method="post" enctype="multipart/form-data">
        <input type="file" name="file" id="file">
        <br>
        <input type="submit" name="submit" value="Upload File">
    </form>
</body>
</html>

```
Que tendrá la siguiente estética:
![[Pasted image 20240326152306.png]]
Y luego un upload.php que procesará la subida del archivo:
```php
<!DOCTYPE html>
<html>
<head>
    <title>Upload File</title>
    <style>
        body {
            background-color: #222;
            color: #fff;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
        }
        h2 {
            margin-top: 50px;
        }
        form {
            margin-top: 20px;
        }
        input[type="file"] {
            border: 2px solid #444;
            border-radius: 5px;
            background-color: #333;
            color: #fff;
            padding: 10px;
            width: 300px;
            max-width: 100%;
            margin-bottom: 20px;
            box-sizing: border-box;
        }
        input[type="submit"] {
            background-color: #4CAF50;
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        input[type="submit"]:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body> 
    <h2>Upload File</h2>
    <form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post" enctype="multipart/form-data">
        <input type="file" name="file" id="file">
        <br>
        <input type="submit" name="submit" value="Upload File">
    </form>

    <?php
    $targetDirectory = "uploads/";
    $targetFile = $targetDirectory . basename($_FILES["file"]["name"]);
    $uploadOk = 1;

    if(isset($_POST["submit"])) {
        if (move_uploaded_file($_FILES["file"]["tmp_name"], $targetFile)) {
            echo "<p>The file " . htmlspecialchars(basename($_FILES["file"]["name"])) . " has been uploaded.</p>";
        } else {
            echo "<p>Error.</p>";
            $uploadOk = 0;
        }
    }
    ?>
</body>
</html>
```
A continuación, probamos en subir una reverse shell y vemos que se ha guardado en el siguiente directorio /uploads:
![[Pasted image 20240326152344.png]]
![[Pasted image 20240326152410.png]]
Una vez dentro como el usuario www-data, si se ejecuta el comando sudo -l, este usuario podrá ejecutar el binario env como el usuario root, pudiendo así escalar privilegios, por lo que editamos el archivo visudo y añadimos lo siguiente:
```bash
sudo visudo
www-data ALL=(root) NOPASSWD: /usr/bin/env
```
![[Pasted image 20240326154358.png]]
De esta forma, si desde el usuario www-data ejecutamos el siguiente comando, nos podríamos convertir en root al ejecutar una bash con privilegios:
```bash
sudo -u root /usr/bin/env /bin/sh -p
```
