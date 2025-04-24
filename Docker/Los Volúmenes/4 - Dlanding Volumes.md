Son aquellos volúmenes que quedan inoperativos y no sirven, es decir, que ningún contenedor los está usando. Para acceder a ellos puedo poner docker volume ls -f dangling=true:
![[Pasted image 20221231200541.png]]
Si utilizo un -q obtengo los id de estos volúmenes:
![[Pasted image 20221231200549.png]]
Y para borrarlos utilizo xargs docker volume rm:
![[Pasted image 20221231200558.png]]
Y ahora como vemos ya no tengo ningún volumen dangling:
![[Pasted image 20221231200612.png]]
