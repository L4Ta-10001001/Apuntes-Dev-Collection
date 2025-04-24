Tenemos el siguiente laboratorio:
![[Pasted image 20240527184252.png]]
En este caso, se nos indica que la web usa Angular, y que, además, existe un DOM based XSS en la funcionalidad de búsqueda. Para completar el laboratorio, tenemos que ejecutar la función alert, por lo que probamos en escribir un mensaje en el buscador:
![[Pasted image 20240527184346.png]]
Y no se ve nada raro, sin embargo, al ver el código fuente, vemos que hay etiqueta llamada ng-app, la cual indica que en este punto comienza a usarse angular:
![[Pasted image 20240527184718.png]]
Por lo que vemos que podemos inyectar un payload compatible con angular:
![[Pasted image 20240527184857.png]]
Usamos el siguiente payload:
```bash
{{constructor.constructor('alert(1)')()}}
```
Y se habrá ejecutado el payload:
![[Pasted image 20240527185050.png]]
