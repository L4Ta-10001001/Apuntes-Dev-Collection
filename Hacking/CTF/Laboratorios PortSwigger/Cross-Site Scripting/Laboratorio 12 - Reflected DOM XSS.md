Este sería el laboratorio:
![[Pasted image 20240529193457.png]]
Y tenemos un buscador:
![[Pasted image 20240529193543.png]]
Ponemos algo en el buscador y lo interceptamos con burp suite:
![[Pasted image 20240529193702.png]]
Tras enviar esta petición, se nos viene la siguiente:
![[Pasted image 20240529193749.png]]
Si enviamos esta petición, vemos que el servidor nos devuelve un json:
![[Pasted image 20240529193828.png]]
Pero podemos ver lo que hemos escrito dentro de la respuesta:
![[Pasted image 20240529194120.png]]
Visto esto, lo que vamos a hacer es escapar el JSON para inyectar el XSS, donde usaremos el siguiente payload:
```bash
test\"-alert(1)}//
```
![[Pasted image 20240529194530.png]]
Enviamos la petición y ha funcionado:
![[Pasted image 20240529194553.png]]
