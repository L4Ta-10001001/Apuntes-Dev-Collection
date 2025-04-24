Un reverse proxy (o proxy inverso) en Apache es un servidor que se sitúa entre los clientes (navegadores web) y los servidores web backend (donde realmente están alojadas las aplicaciones o sitios web). El reverse proxy acepta las solicitudes de los clientes, las reenvía al servidor backend apropiado y luego devuelve la respuesta del servidor backend al cliente.

- **Seguridad**: El reverse proxy puede ocultar la identidad y la estructura de los servidores backend, añadiendo una capa de protección.
- **Balanceo de carga**: Distribuye las solicitudes entrantes entre varios servidores backend, mejorando el rendimiento y la disponibilidad.
- **Caché**: Puede almacenar en caché las respuestas de los servidores backend, reduciendo la carga en estos y mejorando los tiempos de respuesta para los clientes.
- **SSL/TLS Offloading**: Puede manejar la terminación SSL/TLS, aliviando a los servidores backend de esta carga.
- **Compresión**: Puede comprimir las respuestas para reducir el uso de ancho de banda.

-----------------------

Lo primero será configurar un frontend que será el index.html con un botón que llama a una función llamada autenticate().

```html
<!DOCTYPE html>
<html>
<head>
    <title>Mi Sitio</title>
    <script src="authentication.js"></script>
</head>
<body>
    <h1>Bienvenido a Mi Sitio</h1>
    <button onclick="autenticate()">Boton en fase beta</button>
</body>
</html>
```
![[Pasted image 20240729150241.png]]
Y dentro del mismo directorio crearemos un archivo .js que será la función que se ejecutará mediante la consola del navegador al hacerle clic:
![[Pasted image 20240729150323.png]]
Ahora vamos a crear un directorio llamado backend que contendrá el archivo server.js que se encargará de procesar la solicitud del frontend, pero primero iniciamos el proyecto de node.js:
```bash
npm init -y
nmp install express # para instalar el framework de Node.js llamado Express.
```
![[Pasted image 20240729150416.png]]
A continuación, dentro de /etc/apache2/sites-available, vamos a crear la configuración del sitio, que en mi caso le llamaré consolelog.conf, pero previamente instalando los módulos de proxy y proxy_http:
```bash
a2enmod proxy
a2enmod proxy_http
```
Y ahora configuramos el archivo consolelog.conf:
```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName consolelog.lab
    DocumentRoot /var/www/html/sitio

    <Directory /var/www/html/sitio>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ProxyPass /recurso/ http://consolelog.lab:3000/recurso/
    ProxyPassReverse /recurso/ http://consolelog.lab:3000/recurso/

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
![[Pasted image 20240729150548.png]]
Y ahora ejecutamos los siguientes comandos:
```bash
sudo a2ensite consolelog.lab
sudo a2dissite 000-default
sudo systemctl reload apache2
```
Y ponemos el dominio dentro del /etc/hosts de nuestra máquina atacante:
![[Pasted image 20240729150701.png]]
Una vez hecho todo esto, ejecutamos el backend:
```bash
node server.js
```
![[Pasted image 20240729151017.png]]
Y ahora desde el cliente se podrá hacer la petición al frontend, este la hará al reverse proxy y este último al backend:
```bash
curl -X POST http://consolelog.lab:3000/recurso/ -H "Content-Type: application/json" -d '{"token": "tokentraviesito"}'
```
![[Pasted image 20240729151105.png]]
