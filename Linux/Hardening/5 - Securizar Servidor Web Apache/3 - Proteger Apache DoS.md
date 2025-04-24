Podemos antes de nada instalar la herramienta hping3 para hacer ataques DoS:
```bash
sudo apt-get install hping3
```
Y a continuación lanzamos el ataque contra nuestro servidor:
```bash
hping3 -S -p 80 --flood localhost
```

## OPCIÓN 1
Si queremos evitar que nos ataquen el servidor, podemos añadir lo siguiente dentro de nuestro archivo de configuración de apache:
```bash
<IfModule mpm_prefork_module>
    MaxRequestWorkers 150
    ServerLimit 150
</IfModule>
```
## OPCIÓN 2
También podemos instalar el módulo de libapache2-mod-evasive:
```bash
sudo apt-get install libapache2-mod-evasive
```
Y editamos el archivo de configuración `etc/apache2/mods-available/evasive.conf`:
```bash
DOSHashTableSize 3097
DOSPageCount 20
DOSSiteCount 100
DOSSiteInterval 1
DOSBlockingPeriod 10
```
Y lo activamos:
```bash
sudo a2enmod evasive
```
## OPCIÓN 3
También podemos activar un fail2ban:
```bash
sudo apt-get install fail2ban
```
