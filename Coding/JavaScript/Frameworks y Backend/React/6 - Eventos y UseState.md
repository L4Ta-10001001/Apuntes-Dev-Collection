Los eventos sirven para que los componentes puedan reaccionar ante la interacción del usuario (un clic, escribir, etc).

------

En primer lugar, vamos a crear un botón de la siguiente forma, donde definimos un nuevo archivo que llamaremos ContadorApp.jsx dentro del main.jsx:
![[Pasted image 20240708100308.png]]
Y ahora añadimos el siguiente contenido dentro del ContadorApp.jsx:
![[Pasted image 20240708100433.png]]
Y esto veríamos en el navegador:
![[Pasted image 20240708100454.png]]
Aunque también podemos conseguir lo mismo de esta forma, usando una etiqueta button:
![[Pasted image 20240708100552.png]]
Podemos añadir un evento a este botón, por ejemplo que en la consola se imprima un mensaje cada vez que le hagamos clic:
![[Pasted image 20240708100745.png]]
![[Pasted image 20240708100807.png]]
También podemos definir una función aparte y al hacer clic al botón llamar a dicha función, incluso pasándole varios argumentos, donde en este caso pasaremos el evento event (que dará mucha información) y un argumento en forma de texto:
![[Pasted image 20240708101654.png]]
Y ahora si hacemos clic en el botón, veremos que ocurren dos eventos:
![[Pasted image 20240708101720.png]]
Por ejemplo, ahora vamos a crear una función que reciba un 0 y cada vez que hagamos clic al botón se vaya sumando, por lo que primero desde main.jsx vamos a pasar un 0 mediante la variable value:
![[Pasted image 20240708102533.png]]
Y ahora dentro del contador.jsx lo procesamos enviando dicho valor a la función manejarClick:
![[Pasted image 20240708102609.png]]
Y vemos que habrá funcionado:
![[Pasted image 20240708102631.png]]
Pero vemos que en la web no se refleja este cambio de estado, y para ello se utilizan los UseState:
# USESTATE
Para reflejar el cambio en la pantralla, debemos primero recibir el value por parte del padre main.jsx, para después crear una constante que utilice el método setContador y con useState recibir el número 0, para a continuación sumarle un 1 a cada clic usando el método setContador:
![[Pasted image 20240708104729.png]]
Y ahora vemos que funciona:
![[Pasted image 20240708104743.png]]
