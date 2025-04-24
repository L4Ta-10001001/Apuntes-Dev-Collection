En el enunciado nos comentan que tenemos un botón de submit feedback donde se acontece un XSS:
![[Pasted image 20240524102854.png]]
Una vez accedemos, vemos que se nos añade el parámetro:
```bash
returnPath=/
```
![[Pasted image 20240524102942.png]]
Podemos probar en escribir algo:
![[Pasted image 20240524103348.png]]
Y si pasamos el cursos por encima del Back vemos que se refleja:
![[Pasted image 20240524103803.png]]
Es decir, vemos que se refleja lo que escribimos dentro del código fuente:
![[Pasted image 20240524103847.png]]
Por lo que si usamos el siguiente payload, habremos explotado el XSS:
![[Pasted image 20240524104111.png]]
