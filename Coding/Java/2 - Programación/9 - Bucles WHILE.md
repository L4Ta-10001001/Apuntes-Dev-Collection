En Java, el bucle `while` se utiliza para repetir un bloque de código mientras una condición sea verdadera. La estructura básica del bucle `while` es la siguiente:
```java
public class Main {
    public static void main(String[] args) {
        int contador = 1;

        while (contador <= 5) {
            System.out.println("Número: " + contador);
            contador++;
        }
    }
}
```
![[Pasted image 20240304194708.png]]
En este ejemplo, el bucle `while` se ejecutará mientras la variable `contador` sea menor o igual a 5. En cada iteración, se imprime el valor de `contador` y se incrementa en 1. El bucle terminará cuando `contador` alcance el valor 6.