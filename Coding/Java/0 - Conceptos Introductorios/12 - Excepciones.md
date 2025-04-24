Las excepciones se utilizan para manejar errores o situaciones excepcionales que pueden ocurrir durante la ejecución de un programa, como intentar acceder a un archivo que no existe, dividir por cero, entre otros.

1. **`try`**: El bloque de código que puede lanzar una excepción se coloca dentro de un bloque `try`.
    
2. **`catch`**: Bloque que captura y maneja la excepción lanzada. Puedes tener múltiples bloques `catch` para manejar diferentes tipos de excepciones.
    
3. **`finally`**: Bloque opcional que siempre se ejecuta después del bloque `try` (y después de los bloques `catch` si los hay), independientemente de si se lanzó una excepción o no. Es útil para liberar recursos como cerrar archivos o conexiones de red.
    
4. **`throw`**: Se utiliza para lanzar una excepción de forma explícita.

--------------

Por ejemplo, tendremos el siguiente código:
```java
public class Main {
    public static void main (String[] args) {
        try {
            int resultado = calcular(10,0);
            System.out.println(resultado);
        }

        catch (ArithmeticException error) {
            System.out.println("No se puede dividir");
        }

        finally {
            System.out.println("Esto siempre se ejecutará");
        }
}

public static int calcular(int a, int b) {
    return a / b;
    }
}
```
![[Pasted image 20240519213956.png]]
## THROW
El uso de `throw` en Java permite lanzar una excepción de forma explícita desde el código. Esto es útil cuando deseas generar una excepción específica bajo ciertas condiciones en tu programa.

-------------

```java
public class EjemploThrow {
    public static void main(String[] args) {
        try {
            int resultado = dividir(10, 0);
            System.out.println("Resultado: " + resultado);
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    public static int dividir(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("División por cero no permitida.");
        }
        return a / b;
    }
}
```
O también para lanzar una excepción personalizada:
```java
public class EjemploExcepcionPersonalizada {
    public static void main(String[] args) {
        try {
            metodoQueLanzaMiExcepcion();
        } catch (MiExcepcionPersonalizada e) {
            System.out.println("Capturada mi excepción personalizada: " + e.getMessage());
        }
    }

    public static void metodoQueLanzaMiExcepcion() throws MiExcepcionPersonalizada {
        throw new MiExcepcionPersonalizada("Este es un mensaje de excepción personalizada.");
    }
}
```
