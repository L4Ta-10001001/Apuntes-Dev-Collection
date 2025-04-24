- Todo programa Java debe estar siempre dentro de una clase, donde cada clase tendrá una llave de apertura y otra de cierre:
 ![[Pasted image 20240305170850.png]]
Y dentro escribimos una sentencia que va a imprimir el texto Hola; y al tratarse de una sentencia, siempre deben terminar con un punto y coma:
![[Pasted image 20240305172954.png]]

## CONSTRUCTOR Y MÉTODOS 

### Métodos:

Los métodos en Java son bloques de código que realizan una tarea específica. Puedes pensar en ellos como pequeñas acciones que puede realizar un objeto. Cada método tiene un nombre y puede aceptar ciertos datos llamados parámetros, y también puede devolver un resultado si es necesario.

Por ejemplo, imagina que tienes una clase llamada "Perro" y quieres que este perro pueda ladrar. Puedes crear un método llamado "ladrar()" dentro de la clase "Perro", y cuando lo llames, el perro hará el sonido de ladrido.

### Constructores:

Los constructores en Java son métodos especiales que se llaman automáticamente cuando se crea un objeto de una clase. Su principal función es inicializar los valores de los atributos del objeto o realizar otras tareas necesarias para preparar el objeto para su uso.

Por ejemplo, supongamos que tienes una clase llamada "Persona" con un atributo "nombre". Puedes crear un constructor para inicializar el nombre cuando se crea un objeto de la clase "Persona".
```java
public class Persona {

    private String nombre;
    private int edad;

    // Constructor
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    // Método para imprimir información de la persona
    public void imprimirInformacion() {
        System.out.println("Nombre: " + nombre);
        System.out.println("Edad: " + edad);
    }

    // Método principal para probar la clase Persona
    public static void main(String[] args) {
        // Crear instancias de Persona y llamar al método para imprimir información
        new Persona("Juan", 30).imprimirInformacion();
        new Persona("Mario", 28).imprimirInformacion();
    }
}
```
**Otro ejemplo pero creando y eliminando una carpeta**
```java
import java.io.File;

public class ManejoCarpetas {
    private File carpeta;

    // Constructor
    public ManejoCarpetas(String nombreCarpeta) {
        this.carpeta = new File(nombreCarpeta);
    }

    // Método para crear una carpeta
    public void crearCarpeta() {
        carpeta.mkdir();
        System.out.println("Se creó la carpeta: " + carpeta);
    }

    // Método para eliminar una carpeta
    public void eliminarCarpeta() {
        carpeta.delete();
        System.out.println("Se eliminó la carpeta: " + carpeta);
    }

    // Método principal para probar la clase ManejoCarpetas
    public static void main(String[] args) {
        // Crear una instancia de ManejoCarpetas utilizando el constructor
        ManejoCarpetas carpeta = new ManejoCarpetas("MiCarpeta");

        
        carpeta.crearCarpeta(); // Llamar al método para crear la carpeta
        carpeta.eliminarCarpeta(); // Llamar al método para eliminar la carpeta
    }
}
```

--------------

**INSTANCIAR UNA CLASE**
Lo primero creamos la clase coche con su constructor donde tendremos sus atributos. En este código, el constructor es el método `coche()`:
```java
package com.programacion;

public class coche {
    int ruedas;
    int largo;
    int ancho;
    int motor;
    int peso;

    public coche(){ // Constructor

        ruedas=4;
        largo=2000;
        ancho=300;
        motor=1600;
        peso=500;

    }
}
```
Y ahora, desde otro archivo, haremos la instancia de clase para acceder a estos atributos:
```java
package com.programacion;

public class uso_coche {
    public static void main(String[] args) {
        coche Renault=new coche(); // Esto es una instancia de clase.
        System.out.println("Este coche pesa " + Renault.peso);
    }

}
```
De esta forma, si desde este último código lo ejecutamos, estaremos accediendo al atributo del coche peso:
![[Pasted image 20240319155252.png]]
### CLASE ABSTRACTA
Una clase abstracta en Java es una clase que no puede ser instanciada directamente, es decir, no puedes crear objetos de esa clase. Su propósito principal es servir como una base para otras clases. Se utiliza cuando quieres definir una estructura común para un grupo de clases relacionadas, pero no quieres proporcionar una implementación completa para todos los métodos en esa estructura.
#### Características de una clase abstracta

1. **Declaración**: Se declara con la palabra clave `abstract`.
2. **Métodos abstractos**: Puede tener métodos abstractos, que son métodos sin implementación (sin cuerpo). Las subclases que heredan de la clase abstracta deben proporcionar la implementación de estos métodos.
3. **Métodos concretos**: También puede tener métodos concretos, es decir, métodos con implementación.
4. **Herencia**: Las clases que extienden una clase abstracta deben proporcionar implementaciones para los métodos abstractos o también pueden ser declaradas como abstractas.
```java
// Definir una clase abstracta llamada Animal
abstract class Animal {
    // Método abstracto (sin implementación)
    abstract void hacerSonido();

    // Método concreto (con implementación)
    void dormir() {
        System.out.println("El animal está durmiendo.");
    }
}

// Definir una subclase concreta llamada Perro
class Perro extends Animal {
    // Implementar el método abstracto
    @Override
    void hacerSonido() {
        System.out.println("El perro ladra.");
    }
}

// Definir otra subclase concreta llamada Gato
class Gato extends Animal {
    // Implementar el método abstracto
    @Override
    void hacerSonido() {
        System.out.println("El gato maúlla.");
    }
}

// Clase principal para probar las clases abstractas y sus subclases
public class Main {
    public static void main(String[] args) {
        // No se puede crear una instancia de Animal
        // Animal animal = new Animal(); // Esto causaría un error

        // Crear instancias de Perro y Gato
        Animal miPerro = new Perro();
        Animal miGato = new Gato();

        // Usar los métodos
        miPerro.hacerSonido(); // Output: El perro ladra.
        miPerro.dormir();      // Output: El animal está durmiendo.
        
        miGato.hacerSonido();  // Output: El gato maúlla.
        miGato.dormir();       // Output: El animal está durmiendo.
    }
}
```

### Explicación del código

1. **Clase Abstracta `Animal`**:
    
    - La clase `Animal` es abstracta y contiene un método abstracto `hacerSonido` que debe ser implementado por las subclases.
    - También tiene un método concreto `dormir` que imprime un mensaje.
2. **Subclase `Perro`**:
    
    - Extiende `Animal` y proporciona la implementación del método `hacerSonido`, que imprime "El perro ladra."
3. **Subclase `Gato`**:
    
    - Extiende `Animal` y proporciona la implementación del método `hacerSonido`, que imprime "El gato maúlla."
4. **Clase Principal `Main`**:
    
    - En el método `main`, se crean instancias de `Perro` y `Gato`.
    - Se llaman a los métodos `hacerSonido` y `dormir` para cada instancia, demostrando cómo funcionan las subclases que heredan de la clase abstracta `Animal`.

Este ejemplo muestra cómo se puede utilizar una clase abstracta en Java para definir una estructura común y asegurar que las subclases implementen ciertos métodos.

---------------

