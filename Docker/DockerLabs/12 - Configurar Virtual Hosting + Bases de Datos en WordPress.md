Cuando montamos un wordpress, y después exportamos la máquina, podemos encontrarnos con este problema al momento de apuntar a una IP:
![[Pasted image 20240516132152.png]]
Para solucionar esto, primero tenemos que crear un virtualhosting de la siguiente forma, donde supongamos que por ejemplo el dominio será http://collections.dl
![[Pasted image 20240516132302.png]]
Escribimos el siguiente contenido:
```bash
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin admin@collections.dl
        DocumentRoot "/var/www/html/"
        ServerName collections.dl
        ServerAlias www.collections.dl
        <Directory /var/www/html/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
        </Directory>


        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```
Para aplicar estos cambios, utilizamos los siguientes comandos:
```bash
a2ensite nombre.conf 
systemctl reload apache2
a2enmod rewrite
systemctl restart apache2
apachectl -t
```
Y a continuación, entramos dentro de la base de datos y debemos de decir que la vieja IP apunte contra el nuevo dominio configurado anteriormente. La IP 172.17.0.3 es la IP vieja, y le ponemos que apunte al dominio:
```bash
use wordpress;

UPDATE wp_options SET option_value = REPLACE(option_value, 'http://172.17.0.3', 'http://collections.dl');
```
![[Pasted image 20240516132442.png]]
Guardamos los datos:
```bash
FLUSH PRIVILEGES; EXIT;
```
![[Pasted image 20240516132506.png]]
Ahora ya funciona correctamente:
![[Pasted image 20240516132818.png]]
