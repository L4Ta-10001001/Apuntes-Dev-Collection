Un array consiste en una estructura de datos que contiene una colección de valores del mismo tipo; y sirve para almacenar valores que normalmente tienen una relación entre sí.

Esta sería la sintaxis básica donde creamos un array que contenga 5 elementos como máximo:
```java
int[] mi_matriz_new int[5]
```
Para poder ir escribiendo elementos en el array, lo hacemos de la siguiente forma y luego accedemos a ellos mediante índices:
```java
public class Main {

    public static void main(String[] args) {

        int[] mi_array=new int[5];
        
        mi_array[0]=5;
        mi_array[1]=38;
        mi_array[2]=40;
        mi_array[3]=92;
        mi_array[4]=71;

        System.out.println("El valor de la posición 3 del array es: " + mi_array[3]);

    }

}
```
![[Pasted image 20240319110308.png]]
También podemos crear el array al momento de su declaración, de la siguiente forma:
```java
public class Main {

    public static void main(String[] args) {

        int[] mi_array= {1,2,3,4,5};
        
        System.out.println("El valor de la posición 3 del array es: " + mi_array[3]);

    }

}
```
![[Pasted image 20240319110430.png]]
Incluso podemos también ir recorriendo el array mediante un bucle for:
```java
public class codigo {

    public static void main(String[] args) {

        // Declarar e inicializar el array
        int[] numeros = {1, 2, 3, 4, 5};

        // Recorrer el array e imprimir cada número
        for (int numero : numeros) {
            System.out.println("En esta vualta, la variable número vale: " + numero);
        }

    }
}
```
![[Pasted image 20240319110455.png]]
## ArrayList
Las `ArrayList` en Java son una implementación de la interfaz `List` que permite almacenar elementos de manera dinámica.

```java
import java.util.ArrayList;

class Nombres {
    private ArrayList<String> nombres;

    public Nombres() {
        this.nombres = new ArrayList<>();
    }

    public ArrayList<String> getNombres() {
        return nombres;
    }

    public void agregarNombre(String nombre) {
        nombres.add(nombre);
    }
}

public class EjemploArrayList {
    public static void main(String[] args) {
        Nombres nombres = new Nombres();

        // Agregar elementos al ArrayList
        nombres.agregarNombre("Alice");
        nombres.agregarNombre("Bob");
        nombres.agregarNombre("Charlie");

        // Imprimir el ArrayList
        System.out.println("Nombres: " + nombres.getNombres());
    }
}
```
![[Pasted image 20240520093440.png]]
