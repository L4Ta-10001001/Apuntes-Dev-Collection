Hacemos el escaneo de nmap, pero vemos que nos encuentra 2 puertos y la intrusión debería ir por otro puerto que no está encontrando:
![[Pasted image 20240107201948.png]]
Esto ocurre porque debemos identificar la IPv6 de la máquina víctima; y para ello ejecutamos el siguiente comando para encontrar todos los equipos conectados dentro de mi red privada pero con su IPv6:
```bash
ping6 -c2 -n -I eth0 ff02::1
```
![[Pasted image 20240107202038.png]]
