El primer paso será tener nuestro VPS donde alojaremos la web:
![[Pasted image 20241005151444.png]]
Creamos el sitio en apache dentro de /etc/apache2/sites-available:
```bash
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        ServerName nereagonzalez.art
        DocumentRoot /var/www/html

        <Directory /var/www/html>
           Options Indexes FollowSymLinks
           AllowOverride All
           Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Y lo habilitamos:
```bash
sudo a2ensite 000-default.conf
```
![[Pasted image 20241005151948.png]]
Y a continuación debemos de crear un registro A para que el dominio apunte contra la IP del vps:
![[Pasted image 20241005091544.png]]
![[Pasted image 20241005091613.png]]
A continuación, debemos activar el certificado SSL para que nuestra web cuente con HTTPS, por lo que usaremos certbot:
```bash
https://certbot.eff.org/
```
![[Pasted image 20240730115057.png]]
Ponemos el sistema y servicio donde vayamos a instalar el certificado:
![[Pasted image 20240730115131.png]]
Y tras elegir el sistema, se nos despliegan una serie de instrucciones:
![[Pasted image 20240730115257.png]]
Y ahora vamos a instalar snap:
```bash
apt install snapd
systemctl enable --now snapd
```

```bash
a2ensite dockerlabs.conf
systemctl reload apache2
```
![[Pasted image 20240730125543.png]]
Y ahora a continuación instalamos certbot y a continuación creamos un enlace simbólico de certbot::
```bash
sudo snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
```
![[Pasted image 20240730125707.png]]
Ahora solo tenemos que ejecutar el siguiente comando y si el dominio está apuntando contra la IP, ya tendremos HTTPS:
```bash
sudo certbot --apache
```
![[Pasted image 20241005153317.png]]
Y ahora ya tendremos HTTPS:
![[Pasted image 20241006093912.png]]
![[Pasted image 20241006094521.png]]
Ahora por último tenemos que hacer que el certificado se renueve solo automáticamente:
```bash
sudo certbot renew --dry-run
```
![[Pasted image 20241006094612.png]]

