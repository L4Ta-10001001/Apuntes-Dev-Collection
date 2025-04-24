Lo primero será tener nuestra carpeta /public creada:
![[Pasted image 20240909171323.png]]
Y a continuación debemos editar el archivo 000-default.conf que es el archivo donde se aplican las configuraciones por defecto de apache, y pondremos lo siguiente:
```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/public

    <Directory /var/www/html/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
![[Pasted image 20240909171601.png]]
Y después otorgaremos los siguientes permisos a la carpeta /public:
```bash
chown -R www-data:www-data public
chmod -R 755 /var/www/html/public
```
De esta forma, ahora todos los archivos que queramos que sean visibles desde el exterior estarán en el directorio /public:
![[Pasted image 20240909171749.png]]
![[Pasted image 20240909171804.png]]
