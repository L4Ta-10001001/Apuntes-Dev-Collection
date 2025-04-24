Tenemos el siguiente laboratorio, donde vemos un input para insertar un dato y se reflejará en la pantalla:
![[Pasted image 20240416122220.png]]
Si escribo Mario, se refleja:
![[Pasted image 20240416122238.png]]
Dando el siguiente resultado:
![[Pasted image 20240416122251.png]]
Pero si insertamos un script de alert, saldrá en la pantalla porque esto no está sanitizado:
```bash
<script>alert(name)</script>
```
![[Pasted image 20240416122330.png]]
