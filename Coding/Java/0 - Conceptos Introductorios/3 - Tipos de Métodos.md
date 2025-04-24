En Java, los métodos se pueden clasificar en diferentes tipos según su comportamiento y cómo interactúan con los objetos de una clase. Aquí hay una descripción de algunos tipos comunes de métodos en Java.

---------

**MÉTODOS ESTÁTICOS**: En Java, los métodos estáticos se pueden asemejar a funciones en otros lenguajes de programación. Los métodos estáticos son aquellos que pertenecen a la clase en lugar de a una instancia específica de la clase. Esto significa que puedes invocar un método estático sin necesidad de crear un objeto de la clase.
```java
public class ClaseEjemplo {
    private static int contador = 0;
    
    public static void metodoEstatico() {
        contador++;
        System.out.println("Contador: " + contador);
    }
    
    public static void main(String[] args) {
        ClaseEjemplo.metodoEstatico(); // Invocación directa del método estático de la clase ClaseEjemplo
        ClaseEjemplo.metodoEstatico(); // Invocación directa del método estático de la clase ClaseEjemplo
    }
}
```

**MÉTODOS DE INSTANCIAS**: Son métodos que para ser invocados necesitan tener un objeto de esa clase previamente creado. Por ejemplo, en este caso vamos a crear los objetos objeto1 y objeto2:
```java
public class Ejemplo {
    private int contadorInstancia = 0;
    private static int contadorEstatico = 0;

    // Método de instancia
    public void metodoDeInstancia() {
        contadorInstancia++;
        System.out.println("Contador de instancia: " + contadorInstancia);
    }

    // Método estático
    public static void metodoEstatico() {
        contadorEstatico++;
        System.out.println("Contador estático: " + contadorEstatico);
    }

    public static void main(String[] args) {
        Ejemplo objeto1 = new Ejemplo();
        Ejemplo objeto2 = new Ejemplo();

        // Invocar métodos de instancia a través de objetos
        objeto1.metodoDeInstancia(); // Imprime "Contador de instancia: 1"
        objeto2.metodoDeInstancia(); // Imprime "Contador de instancia: 1" (cada objeto tiene su propia instancia de contador)

        // Invocar método estático directamente en la clase Ejemplo
        Ejemplo.metodoEstatico(); // Imprime "Contador estático: 1"
        Ejemplo.metodoEstatico(); // Imprime "Contador estático: 2" (compartido por todas las instancias de la clase)
    }
}
```
**Métodos de Clase o Métodos de Fábrica**: Son métodos estáticos utilizados para crear instancias de la clase o realizar alguna operación que no depende del estado de ningún objeto:
```java
public class MiClase {
    public static MiClase crearInstancia() {
        return new MiClase();
    }
}

MiClase objeto = MiClase.crearInstancia(); // Uso de un método de clase para crear una instancia
```
**Métodos de Acceso (Getters) y Modificación (Setters)**: Son métodos utilizados para obtener (`get`) y establecer (`set`) el valor de las variables de instancia de una clase [[5 - Getters y Setters]].
```java
public class Persona {
    private String nombre;

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nuevoNombre) {
        this.nombre = nuevoNombre;
    }
}

Persona persona = new Persona();
persona.setNombre("Juan"); // Establecer el nombre
System.out.println(persona.getNombre()); // Obtener el nombre
```
**Métodos Constructores**: Son métodos especiales utilizados para inicializar un objeto recién creado. Tienen el mismo nombre que la clase y no tienen tipo de retorno.
```java
public class Persona {
    private String nombre;

    // Constructor
    public Persona(String nombre) {
        this.nombre = nombre;
    }
}

Persona persona = new Persona("Juan"); // Crear un objeto utilizando el constructor
```
**Métodos de Sobrecarga**: Son métodos que tienen el mismo nombre pero diferentes listas de parámetros. Java permite definir varios métodos con el mismo nombre siempre que tengan diferentes firmas de parámetros.
```java
public class Operaciones {
    public int sumar(int a, int b) {
        return a + b;
    }

    public double sumar(double a, double b) {
        return a + b;
    }
}

Operaciones operaciones = new Operaciones();
int sumaEnteros = operaciones.sumar(3, 5); // Invocación del método sumar con enteros
double sumaDobles = operaciones.sumar(3.5, 5.2); // Invocación del método sumar con dobles
```