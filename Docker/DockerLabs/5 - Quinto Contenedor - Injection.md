
La idea será de esta máquina vulnerable es que cuente con los puertos 22 y 80 abiertos. De tal forma que en la página web haya un formulario de login donde se tenga que insertar un usuario y contraseña. Si la contraseña es correcta, se mostrará un mensaje por pantalla diciendo que el usuario con la contraseña es correcto. Por tanto, este formulario será vulnerable a una inyección SQL básica donde se podrá ver todos los usuarios registrados en la base de datos.

Una vez obtenidos todos los usuarios, sólo uno de ellos será válido para acceder por SSH, por lo que se tendrá que utilizar Hydra para encontrar el usuario válido.

Una vez dentro, la contraseña del usuario root será débil, por lo que se deberá de utilizar algún script para realizar un ataque de fuerza bruta para encontrar dicha contraseña y escalar a root con el comando sudo su, pudiendo utilizar el siguiente script de ejemplo:
```bash
https://github.com/Maalfer/Sudo_BruteForce
```

----------

Lo primero será configurar la base de datos:
```bash
apt install mariadb-server
```
Y habilitamos el servicio:
```bash
service mariadb start
service mariadb status
```
![[Pasted image 20240320151417.png]]
Ejecutamos el siguiente comando para configurar la base de datos:
```bash
mysql_secure_installation
```
![[Pasted image 20240320151631.png]]
Nos conectaremos a la base de datos con el siguiente comando y con la contraseña del usuario root que hayamos especificado anteriormente:
```bash
mysql -u root -ppinguinasio123
```
![[Pasted image 20240324171237.png]]
Y creamos la base de datos register donde se almacenarán los usuarios registrados:
![[Pasted image 20240324172808.png]]
Creamos una tabla de usuarios con dos columnas:
```sql
CREATE TABLE users (username VARCHAR(30) NOT NULL PRIMARY KEY, passwd VARCHAR(30) NOT NULL);
```
Y creamos un usuario llamado dylan con la contraseña KJSDFG789FGSDF78 que será la que se muestre después de explotar la SQLi:
![[Pasted image 20240325175653.png]]
Después instalamos lo siguiente:
```bash
apt install php-mysql php-db
php-fpm
libapache2-mod-php
```
Una vez la base de datos esté configurada, podemos instalar el servidor de apache:
```bash
apt install apache2
```
![[Pasted image 20240324171337.png]]
Lo primero será crear un formulario de index.php
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Iniciar Sesión</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        /* Estilos adicionales */
        body {
            font-family: Arial, sans-serif;
            background-color: #1f1f1f; /* Color de fondo oscuro */
            color: #ffffff; /* Color del texto */
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center; /* Centra horizontalmente */
            align-items: center; /* Centra verticalmente */
            height: 100vh; /* Ajusta la altura para ocupar toda la pantalla */
        }

        .background {
            background-color: #333333; /* Color de fondo del recuadro */
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0px 0px 10px rgba(255, 255, 255, 0.1); /* Sombra */
            width: 80%;
            max-width: 400px;
        }

        h2 {
            text-align: center;
            color: #ffffff; /* Color del título */
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            color: #ffffff; /* Color de las etiquetas */
        }

        input[type="text"],
        input[type="password"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #555555; /* Color del borde del campo de entrada */
            border-radius: 5px;
            background-color: #222222; /* Color de fondo del campo de entrada */
            color: #ffffff; /* Color del texto dentro del campo de entrada */
        }

        button[type="submit"] {
            width: 100%;
            padding: 10px;
            border: none;
            border-radius: 5px;
            background-color: #007bff; /* Color de fondo del botón */
            color: #ffffff; /* Color del texto del botón */
            cursor: pointer;
            transition: background-color 0.3s;
        }

        button[type="submit"]:hover {
            background-color: #0056b3;
        }

        .error-message {
            color: red;
            text-align: center;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
<?php
session_start();
$_SESSION['user'] = "";
$forward = "Location: /index.php";

if (isset($_POST['submit'])) {
        $result = [
                'error' => true,
                'msg' => 'Wrong Credentials'
        ];

        $_SESSION['result'] = $result;

        $config = include 'config.php';

        try {
                $dsn = 'mysql:host=' . $config['db']['host'] . ';dbname=' . $config['db']['dbname'];
                $conet = new PDO($dsn, $config['db']['user'], $config['db']['passwd'], $config['db']['options']);

                $consult = "SELECT * FROM users WHERE username = '" . $_POST['name'] . "' AND passwd = '" . $_POST['password'] . "'";

                $send = $conet->prepare($consult);
                $send->execute();

                if ($send->rowCount() > 0) {
                        $row = $send->fetch();
                        $_SESSION['user'] = $row["username"];
                        $forward = "Location: /acceso_valido_dylan.php";

                        $_SESSION['result']['error'] = false;
                }

        } catch(PDOException $error) {
                $result['error'] = true;
                $result['msg'] = $error->getMessage();
                $_SESSION['result'] = $result;
        }
        header($forward);
}

?>
<?php
if ($_SESSION['result']['error']) {
?>
        <div class="error-message">
                <?= $_SESSION['result']['msg'] ?>
        </div>
<?php
}
?>

    <div class="background">
        <h2>Login</h2>
        <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="post">
            <div class="form-group">
                <label for="username">User:</label>
                <input type="text" id="name" name="name" required>
            </div>
            <div class="form-group">
                <label for="password">Password:</label>
                <input type="password" id="password" name="password" required>
            </div>
            <button type="submit" name="submit" >Login</button>
        </form>
    </div>
</body>
</html>
```
![[Pasted image 20240325175909.png]]
Y un acceso_valido_dylan.php que se mostrará una vez explotada la inyección SQL:
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Iniciar Sesión</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        /* Estilos adicionales */
        body {
            font-family: Arial, sans-serif;
            background-color: #1f1f1f; /* Color de fondo oscuro */
            color: #ffffff; /* Color del texto */
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center; /* Centra horizontalmente */
            align-items: center; /* Centra verticalmente */
            height: 100vh; /* Ajusta la altura para ocupar toda la pantalla */
        }

        .background {
            background-color: #333333; /* Color de fondo del recuadro */
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0px 0px 10px rgba(255, 255, 255, 0.1); /* Sombra */
            width: 80%;
            max-width: 400px;
        }

        h2 {
            text-align: center;
            color: #ffffff; /* Color del título */
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            color: #ffffff; /* Color de las etiquetas */
        }

        input[type="text"],
        input[type="password"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #555555; /* Color del borde del campo de entrada */
            border-radius: 5px;
            background-color: #222222; /* Color de fondo del campo de entrada */
            color: #ffffff; /* Color del texto dentro del campo de entrada */
        }

        button[type="submit"] {
            width: 100%;
            padding: 10px;
            border: none;
            border-radius: 5px;
            background-color: #007bff; /* Color de fondo del botón */
            color: #ffffff; /* Color del texto del botón */
            cursor: pointer;
            transition: background-color 0.3s;
        }

        button[type="submit"]:hover {
            background-color: #0056b3;
        }

        .error-message {
            color: red;
            text-align: center;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
<?php
session_start();
$_SESSION['user'] = "";
$forward = "Location: /index.php";

if (isset($_POST['submit'])) {
        $result = [
                'error' => true,
                'msg' => 'Wrong Credentials'
        ];

        $_SESSION['result'] = $result;

        $config = include 'config.php';

        try {
                $dsn = 'mysql:host=' . $config['db']['host'] . ';dbname=' . $config['db']['dbname'];
                $conet = new PDO($dsn, $config['db']['user'], $config['db']['passwd'], $config['db']['options']);

                $consult = "SELECT * FROM users WHERE username = '" . $_POST['name'] . "' AND passwd = '" . $_POST['password'] . "'";

                $send = $conet->prepare($consult);
                $send->execute();

                if ($send->rowCount() > 0) {
                        $row = $send->fetch();
                        $_SESSION['user'] = $row["username"];
                        $forward = "Location: /acceso_valido_dylan.php";

                        $_SESSION['result']['error'] = false;
                }

        } catch(PDOException $error) {
                $result['error'] = true;
                $result['msg'] = $error->getMessage();
                $_SESSION['result'] = $result;
        }
        header($forward);
}

?>
<?php
if ($_SESSION['result']['error']) {
?>
        <div class="error-message">
                <?= $_SESSION['result']['msg'] ?>
        </div>
<?php
}
?>

    <div class="background">
        <h2>Login</h2>
        <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="post">
            <div class="form-group">
                <label for="username">User:</label>
                <input type="text" id="name" name="name" required>
            </div>
            <div class="form-group">
                <label for="password">Password:</label>
                <input type="password" id="password" name="password" required>
            </div>
            <button type="submit" name="submit" >Login</button>
        </form>
    </div>
</body>
</html>
root@db40c69df82d:/var/www/html# ls
acceso_valido_dylan.php  config.php  index.php
root@db40c69df82d:/var/www/html# cat acceso_valido_dylan.php 
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bienvenida</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #1f1f1f;
            color: #ffffff; /* Color del texto */
            margin: 0;
            padding: 0;
        }

        .mensaje {
            background-color: #333333; /* Color de fondo del mensaje */
            width: 80%;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0px 0px 10px rgba(255, 255, 255, 0.1); /* Sombra */
        }

        .mensaje p {
            font-size: 18px;
            line-height: 1.6;
        }
    </style>
</head>
<body>
    <?php
    // Variables
    $nombre = "Dylan";
    $contrasena = "KJSDFG789FGSDF78";
    ?>

    <div class="mensaje">
        <p>Bienvenido <?php echo $nombre; ?>! Has insertado correctamente tu contraseña: <?php echo $contrasena; ?></p>
    </div>
</body>
</html>
```
Dentro del formulario, si utilizamos alguna inyección básica de SQL, deberíamos realizar un bypass y acceder:
```bash
' or 1=1-- -
```
![[Pasted image 20240325175856.png]]
Ahora vamos a crear dentro del sistema el usuario dylan:
```bash
dylan:KJSDFG789FGSDF78
```
![[Pasted image 20240325180049.png]]
E instalamos SSH:
```bash
apt install openssh-server
```
Y ahora vamos a poner una contraseña debil para root para que el atacante tenga que descubrirla mediante el uso de fuerza bruta:
```bash
passwd root
root:superuser
```
![[Pasted image 20240325180233.png]]
Para hacer fuerza bruta de la contraseña, podemos utilizar el siguiente script con el siguiente diccionario:
```bash
https://github.com/carlospolop/su-bruteforce
```
Usándolo de la siguiente forma:
```bash
bash suBF.sh -u root -w top12000.txt -t 0.5
```
