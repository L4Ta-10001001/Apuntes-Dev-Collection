## REQUISITOS PREVIOS

Para que las máquina de DockerLabs puedan funcionar, Docker debe estar instalado en tu sistema:
```bash
sudo apt install docker.io
```
![[Pasted image 20240416200151.png]]
## CÓMO EJECUTAR LAS MÁQUINAS

Una vez hayamos descargado una máquina en formato .tar, veremos que hay un script llamado auto_deploy.sh junto con cada máquina, por lo que solamente tendremos que ejecutar ese script para desplegar o borrar el laboratorio.
```bash
sudo bash auto_deploy.sh laboratorio.tar
```

![[Pasted image 20240416200237.png]]
## CÓMO ELIMINAR LAS MÁQUINAS
Una vez hayamos terminado con el laboratorio, simplemente pulsamos control + C y todo el laboratorio se habrá eliminado del sistema.
![[Pasted image 20240416200301.png]]

## DESPLIEGUE DE ENTORNOS DE PIVOTING
En DockerLabs hay algunos entornos de pivoting para ser desplegados de forma automática tanto de 3 máquinas o más. Por lo que vamos a mostrar cómo desplegar estos entornos:
![[Pasted image 20240421110158.png]]
Debemos de ejecutar el script pasándole como argumento el número de máquinas a desplegar, y de forma automática se crearán todas las redes de docker y sus respectivas configuraciones:
```bash
sudo bash auto_deploy.sh inclusion.tar trust.tar upload.tar walkingcms.tar whereismywebshell.tar
```
![[Pasted image 20240421110334.png]]
De esta forma tendremos conexión únicamente con el primer contenedor (10.10.10.2), pero no con el resto de las máquinas.

Para eliminar todo este entorno de pivoting, simplemente ejecutamos Crtl + C y el script se encargará de limpiarlo todo (Confirmamos que sí para que nos elimine todas las imágenes de docker generadas):
![[Pasted image 20240421110509.png]]
Comprobamos que todo haya sido eliminado correctamente:
![[Pasted image 20240421110534.png]]
## SOLUCIÓN DE ERRORES

Es posible que en algunos casos puntuales se pueda experimentar algún error. Es por ello que proporcionamos una serie de comandos para solucionar la mayoría de errores o problemas que se puedan experimentar:
```bash
1 - sudo systemctl restart docker
2 - sudo docker stop $(docker ps -q)
3 - sudo docker container prune --force
```
