En Java, las funciones se definen mediante la creación de métodos. Un método es un bloque de código que realiza una tarea específica y puede ser invocado (llamado) desde otro lugar en el programa.
```java
public class FuncionesBasicas {

    // Definición de una función para sumar dos números enteros
    public static int sumar(int a, int b) {
        return a + b;
    }

    // Definición de una función para multiplicar dos números enteros
    public static int multiplicar(int a, int b) {
        return a * b;
    }

    public static void main(String[] args) {
        // Llamada a las funciones y almacenamiento del resultado
        int resultadoSuma = sumar(5, 3);
        int resultadoMultiplicacion = multiplicar(4, 6);

        // Impresión de los resultados
        System.out.println("El resultado de la suma es: " + resultadoSuma);
        System.out.println("El resultado de la multiplicación es: " + resultadoMultiplicacion);
    }
}
```
![[Pasted image 20240319104759.png]]
