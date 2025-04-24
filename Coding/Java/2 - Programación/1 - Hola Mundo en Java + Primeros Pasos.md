Para imprimir un hola mundo en Java, necesitamos crear una clase que será el punto de partida; y a continuación siempre tendremos que poner la siguiente instrucción:
```java
public static void main(String[] args) {}
```
Y ya una vez con esto puesto podemos imprimir un mensaje por pantalla:
```java
public class Main {

    public static void main(String[] args) {
        System.out.println("Hola Mundo");
    }
}
```
Y ahora ya podemos imprimir un mensaje por pantalla:
![[Pasted image 20240304164649.png]]
Y si queremos imprimir otro mensaje por pantalla, podemos reutilizar la misma clase y simplemente añadimos otra línea:
```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hola Mundo");
        System.out.println("Otro Mensaje");
    }
}
```
![[Pasted image 20240304165721.png]]
