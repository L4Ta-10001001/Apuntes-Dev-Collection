El polimorfismo permite que un objeto se comporte de diferentes maneras según el contexto en el que se utiliza. Esto se puede lograr mediante dos mecanismos principales en Java: sobrecarga de métodos y sobreescritura de métodos.

## SOBRECARGA DE MÉTODOS
El polimorfismo de sobrecarga de métodos se refiere a la capacidad de tener múltiples métodos con el mismo nombre en una clase, pero con diferentes listas de parámetros:
```java
public class Operaciones {

    // Método para sumar dos enteros
    public int sumar(int a, int b) {
        return a + b;
    }

    // Método sobrecargado para sumar tres enteros
    public int sumar(int a, int b, int c) {
        return a + b + c;
    }

    // Método sobrecargado para sumar dos números decimales
    public double sumar(double a, double b) {
        return a + b;
    }

    public static void main(String[] args) {
        Operaciones operaciones = new Operaciones();

        System.out.println("Suma de 5 y 7: " + operaciones.sumar(5, 7));
        System.out.println("Suma de 5, 7 y 10: " + operaciones.sumar(5, 7, 10));
        System.out.println("Suma de 3.5 y 2.5: " + operaciones.sumar(3.5, 2.5));
    }
}
```

En este ejemplo, la clase `Operaciones` tiene tres métodos llamados `sumar`, pero cada uno acepta diferentes tipos de parámetros. Esto permite utilizar el mismo nombre de método `sumar`, pero realizar operaciones diferentes dependiendo de los parámetros que se le pasen.
## SOBREESCRITURA DE MÉTODOS (polimorfismo de tiempo de ejecución)

El polimorfismo de sobreescritura de métodos se refiere a la capacidad de una subclase para proporcionar una implementación específica de un método que ya está definido en su superclase.

```java
// Clase base o superclase
class Animal {
    public void hacerSonido() {
        System.out.println("Sonido de animal indefinido");
    }
}

class Perro extends Animal {
    // Sobreescritura del método hacerSonido para el sonido de un perro
    @Override
    public void hacerSonido() {
        System.out.println("Guau guau");
    }
}

class Gato extends Animal {
    @Override
    public void hacerSonido() {
        System.out.println("Miau miau");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal1 = new Perro();
        Animal animal2 = new Gato();

        animal1.hacerSonido(); // Imprime "Guau guau"
        animal2.hacerSonido(); // Imprime "Miau miau"
    }
}
```

En este ejemplo, la clase Animal tiene un método hacerSonido() que está diseñado para hacer un sonido genérico de animal. Las clases Perro y Gato son subclases de Animal y cada una de ellas sobrescribe el método hacerSonido() con su propia implementación específica para el sonido de un perro y un gato, respectivamente. Cuando se llama al método hacerSonido() en objetos de tipo Perro y Gato, se ejecuta la implementación específica proporcionada por cada subclase. Esto demuestra el polimorfismo de sobreescritura de métodos en acción:


