El bucle `for` en Java es una estructura de control de flujo que permite repetir un bloque de código un número específico de veces. La sintaxis básica del bucle `for` es la siguiente:
```java
public class Main {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            System.out.println("En esta vuelta, el número vale: " + i);
        }
    }
}
```
La variable i recibe el número 1, y luego decimos que mientras i sea 5 o menos, se le va a ir sumando un 1 a cada vuelta:
![[Pasted image 20240304194111.png]]
#### RECORRIENDO UN ARRAY
También podemos recorrer elementos de un array de la siguiente forma:
```java
public class codigo {
    public static void main(String[] args) {
        String[] nombres = {"Mario", "Fátima", "Eustaquio"}; // Array llamado nombres

        for (String nombre : nombres) { // Recorremos cada elemento del array nombres y lo guardamos en la variable nombre
            System.out.println(nombre);
        }
    }
}
```
![[Pasted image 20240307203752.png]]
