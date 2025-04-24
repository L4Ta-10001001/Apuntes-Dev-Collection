Tenemos todo el código:
![[Pasted image 20240428113524.png]]
Y ejecutamos los siguiente comandos:
```bash
npm install
npm install @angular/cli -g
```
![[Pasted image 20240428113939.png]]
Y ahora ejecutamos el comando ng serve para compilarlo y que el proyecto corra por el puerto 4200:
```bash
ng serve
```
![[Pasted image 20240428114046.png]]
Y ya lo tenemos corriendo por el puerto 4200 de nuestro localhost:
![[Pasted image 20240428114143.png]]
Ahora, si queremos que esto corra por el puerto 80, debemos de ejecutar el comando ng build:
```bash
ng build
```
![[Pasted image 20240428114433.png]]
Al haber ejecutado este comando, veremos que se nos ha creado un directorio llamado dist:
![[Pasted image 20240428114515.png]]
Y navegando por esta carpeta, tendremos el proyecto compilado dentro del directorio /browser, el cual lo podemos pegar en el directorio de apache y levantar la web:
![[Pasted image 20240428114605.png]]
Lo copiamos todo al servidor de apache, en el directorio /var/www/html:
![[Pasted image 20240428114655.png]]
Y ya lo tenemos por aquí:
![[Pasted image 20240428114715.png]]
Y lo tenemos:
![[Pasted image 20240428114728.png]]
### INSTALAR SSL HTTPS EN EL DOMINIO
Para instalar SSL y que la web corra por el puerto 443, por lo que primero instalaremos certbot para generar el certificado:
```bash
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
certbot --apache
```


