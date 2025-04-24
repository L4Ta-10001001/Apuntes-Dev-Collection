Para realizar nuestro primer hola mundo en Angular, debemos instalar primero nodejs y angular CLI:
```bash
apt install nodejs npm
npm install -g @angular/cli
```
![[Pasted image 20240329155915.png]]
Y ahora ya podremos crear nuestro proyecto de angular con el comando ng:
```bash
ng new proyecto
```
![[Pasted image 20240329160014.png]]
Este proceso nos habrá creado la siguiente carpeta:
![[Pasted image 20240329160048.png]]
El siguiente paso será ubicarnos dentro de este directorio y crear un componente, los cuales son bloques de construcción fundamentales en Angular:
```bash
ng generate component prueba
```
Y nos habrá creado una carpeta llamada prueba en /proyecto/src/app:
![[Pasted image 20240329160311.png]]
![[Pasted image 20240329160302.png]]
Podemos editar el archivo HTML y aquí añadir nuestro Hola Mundo:
![[Pasted image 20240329160413.png]]
Ahora si queremos compilar esta aplicación utilizamos el comando ng serve:
```bash
ng serve
```
Y por el puerto 4200 del localhost estará corriendo esta aplicación:
![[Pasted image 20240329160513.png]]
