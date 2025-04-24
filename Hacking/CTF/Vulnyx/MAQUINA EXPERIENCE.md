Hacemos el escaneo con nmap:
![[Pasted image 20240123093622.png]]
Como vemos el puerto 445 abierto y que se trata de una máquina windows xp, vamos a ver mediante el script vuln de nmap si esta máquina es vulnerable o no:
```bash
nmap -p445 --script=vuln 192.168.0.54
```
![[Pasted image 20240123093714.png]]
Buscamos este exploit con metasploit:
![[Pasted image 20240123093834.png]]
Lo seleccionamos y lo lanzamos:
![[Pasted image 20240123094612.png]]
![[Pasted image 20240123094655.png]]
