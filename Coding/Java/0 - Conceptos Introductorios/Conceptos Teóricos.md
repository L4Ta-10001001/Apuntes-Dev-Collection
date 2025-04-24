En Java, los conceptos de sobrecarga (overloading), sobrescritura (overriding) y redefinición de métodos son fundamentales en la programación orientada a objetos. Aquí tienes una explicación detallada de cada uno:
### COMPRENDER LA SOBRECARGA/SOBREESCRITURA/REDEFINICIÓN DE MÉTODOS
### 1. **Sobrecarga de Métodos**
La sobrecarga de métodos en Java se refiere a la creación de varios métodos en la misma clase con el mismo nombre pero con diferentes parámetros (String, int o más de un número):
```java
public class EjemploSobrecarga {
    
    // Método con un parámetro entero
    public void mostrar(int a) {
        System.out.println("Número: " + a);
    }
    
    // Método con dos parámetros enteros
    public void mostrar(int a, int b) {
        System.out.println("Números: " + a + ", " + b);
    }
    
    // Método con un parámetro String
    public void mostrar(String mensaje) {
        System.out.println("Mensaje: " + mensaje);
    }
    
    public static void main(String[] args) {
        EjemploSobrecarga ejemplo = new EjemploSobrecarga();
        ejemplo.mostrar(10);
        ejemplo.mostrar(10, 20);
        ejemplo.mostrar("Hola Mundo");
    }
}
```
En este caso, el método `mostrar` está sobrecargado con tres variantes, cada una con un conjunto diferente de parámetros.

### 2. **Sobrescritura de Métodos (Method Overriding)**

La sobrescritura de métodos permite a una subclase proporcionar una implementación específica de un método que ya está definido en su superclase. Es una forma de polimorfismo en tiempo de ejecución. La sobrescritura se logra utilizando la anotación `@Override` para asegurar que el método está siendo sobrescrito correctamente. Aquí hay un ejemplo:
```java
class Animal {
    public void sonido() {
        System.out.println("El animal hace un sonido");
    }
}

class Perro extends Animal {
    @Override
    public void sonido() {
        System.out.println("El perro ladra");
    }
}

public class EjemploSobrescritura {
    public static void main(String[] args) {
        Animal miAnimal = new Animal();
        Animal miPerro = new Perro();
        
        miAnimal.sonido();  // Salida: El animal hace un sonido
        miPerro.sonido();   // Salida: El perro ladra
    }
}
```
En este caso, el método `sonido` en la clase `Perro` sobrescribe el método `sonido` en la clase `Animal`.

### 3. **Redefinición de Métodos (Method Hiding)**

La redefinición de métodos ocurre cuando una subclase define un método estático con el mismo nombre y firma que un método estático en su superclase. Esto no es lo mismo que sobrescritura, ya que los métodos estáticos no pueden ser sobrescritos. En cambio, el método en la subclase oculta el método en la superclase. Aquí hay un ejemplo:
```java
class Padre {
    public static void mostrar() {
        System.out.println("Método estático en la clase Padre");
    }
}

class Hijo extends Padre {
    public static void mostrar() {
        System.out.println("Método estático en la clase Hijo");
    }
}

public class EjemploRedefinicion {
    public static void main(String[] args) {
        Padre.mostrar();  // Salida: Método estático en la clase Padre
        Hijo.mostrar();   // Salida: Método estático en la clase Hijo
        
        Padre padre = new Hijo();
        padre.mostrar();  // Salida: Método estático en la clase Padre
    }
}
```
En este caso, aunque `Hijo` define un método `mostrar` estático, no sobrescribe el método `mostrar` de `Padre`. En cambio, lo oculta.

***Resumen***

- **Sobrecarga (Overloading)**: Mismo nombre de método, diferente lista de parámetros, dentro de la misma clase.
- **Sobrescritura (Overriding)**: Mismo nombre de método, misma lista de parámetros, pero en clases padre e hijo.
- **Redefinición (Hiding)**: Mismo nombre de método estático, misma lista de parámetros, pero en clases padre e hijo, y el método de la subclase oculta el método de la superclase.

### COMPRENDER LA HERENCIA EN JAVA

[[8 - Herencia]]

### COMPRENDER EL POLIMORFISMO EN JAVA

[[10 - Polimorfismo]]

### SABER CÓMO SINCRONIZAR HILOS

[[12 - Hilos en Java]]

### CONOCER EL PATRÓN MVC Y SUS BENEFICIOS

El patrón MVC (Modelo-Vista-Controlador) es una forma de organizar el código en aplicaciones que separa la lógica (Modelo), la presentación de la información (Vista) y la interacción del usuario (Controlador).

##### Componentes del patrón MVC

- **Modelo**: Define la lógica del contador. Contiene métodos para incrementar, decrementar y resetear el contador, así como para obtener el valor actual del contador.
    
- **Vista**: Define la interfaz gráfica. Contiene etiquetas, campos de entrada y botones. También tiene métodos para actualizar la interfaz con el valor del contador.
    
- **Controlador**: Actúa como intermediario entre el modelo y la vista. Define métodos para manejar eventos de los botones (incrementar, decrementar y resetear) y actualizar la vista.

![[Pasted image 20240630124207.png]]

##### Beneficios del patrón MVC

1. **Separación de responsabilidades**: Cada componente tiene una responsabilidad específica y bien definida, lo que facilita la comprensión y el mantenimiento del código.
    
2. **Facilita el mantenimiento y la escalabilidad**: Al separar la lógica de negocio, la interfaz de usuario y la gestión de entradas del usuario, es más sencillo realizar cambios en una parte de la aplicación sin afectar a las demás.
    
3. **Reutilización de código**: El mismo modelo puede ser reutilizado con diferentes vistas y controladores, lo que permite reutilizar la lógica de negocio en diferentes contextos.
    
4. **Desarrollo paralelo**: Permite que los desarrolladores trabajen en diferentes componentes de la aplicación de manera independiente. Por ejemplo, un desarrollador puede trabajar en la lógica de negocio (Modelo), mientras otro trabaja en la interfaz de usuario (Vista) y un tercero en la lógica de control (Controlador).
    
5. **Mejor organización del código**: El código está más organizado y estructurado, lo que facilita la lectura y el entendimiento del mismo por parte de otros desarrolladores.

### SABER GESTIONAR EXCEPCIONES EN JAVA (try-catch-finally)

[[12 - Excepciones]]

### SABER QUÉ ES UNA SECCIÓN CRÍTICA

En Java, una **sección crítica** es una parte del código que debe ser ejecutada por un solo hilo a la vez para evitar problemas de concurrencia. Los problemas de concurrencia pueden surgir cuando múltiples hilos intentan acceder y modificar los mismos recursos compartidos simultáneamente, lo que puede llevar a resultados inconsistentes o errores.

Solamente un proceso/hilo/subtarea puede estar en un momento dado en la sección crítica de un recurso.

Para implementar una sección crítica en Java, se utilizan diversas técnicas de sincronización, siendo la más común el uso de la palabra clave `synchronized`.

En resumen, una sección crítica en Java es una parte del código que necesita ser ejecutada por un solo hilo a la vez para asegurar la consistencia y evitar problemas de concurrencia, y `synchronized` es la forma más común de implementarla.

### SABER QUÉ ES UN INTERBLOQUEO

Un interbloqueo en Java es una situación en la que dos o más hilos están bloqueados permanentemente, esperando unos por otros para liberar un recurso que necesitan para continuar su ejecución.

Para entender mejor el concepto de interbloqueo, consideremos un ejemplo:

1. **Hilo A** tiene el **Recurso 1** y está esperando para obtener el **Recurso 2**.
2. **Hilo B** tiene el **Recurso 2** y está esperando para obtener el **Recurso 1**.

En esta situación, ninguno de los hilos puede continuar porque cada uno está esperando que el otro libere un recurso que necesita. Esto crea un ciclo de dependencia circular, y como resultado, ambos hilos quedan bloqueados indefinidamente.

### QUÉ ES LA EXCLUSIÓN MUTUA

La exclusión mutua se refiere a la técnica que garantiza que dos o más hilos no accedan simultáneamente a un recurso compartido que puede estar siendo modificado. Esto se logra utilizando mecanismos como los semáforos. 

El objetivo principal de la exclusión mutua es evitar las condiciones de carrera, donde dos o más hilos intentan modificar el mismo recurso compartido simultáneamente, lo que puede llevar a resultados inesperados o inconsistentes.

Solución: garantizar un acceso exclusivo al recurso y una manera de sincronizar este acceso que evite problemas de interbloqueo e inanición.

### SABER QUÉ ES UNA CLASE ABSTRACTA Y CÓMO UTILIZARLA

En Java, una clase abstracta es una clase que no puede ser instanciada directamente; es decir, no se puede crear un objeto de una clase abstracta. Una clase abstracta puede contener tanto métodos abstractos como métodos concretos.

- **Método Concreto**: Declarado con implementación en una clase abstracta. Puede ser utilizado directamente por las subclases o sobrescrito.
- **Clase Abstracta**: No se puede instanciar directamente. Puede contener métodos abstractos y concretos.

Si queremos crear un objeto, no podremos hacerlo sobre la clase abstracta, pero sí podemos hacerlo si creamos una clase que extienda de la abstracta, por ejemplo la clase perro que extiende de animal, o también podemos usar un objeto para llamar al método concreto.

```java
abstract class Animal {
    // Método concreto (con implementación)
    void respirar() {
        System.out.println("El animal está respirando.");
    }
}

class Perro extends Animal {
    void hacerSonido() {
        System.out.println("Guau guau");
    }
}

public class Main {
    public static void main(String[] args) {
        // No se puede instanciar directamente una clase abstracta
        // Animal animal = new Animal(); // Esto causaría un error de compilación

        // Se puede instanciar una clase concreta que extiende la clase abstracta
        Perro perro = new Perro();
        perro.hacerSonido();
        perro.respirar();
    }
}
```

Este diseño permite definir un comportamiento genérico en la clase abstracta (`Animal`), mientras que las subclases (`Perro`) proporcionan comportamientos específicos.

- **No se puede crear un objeto directamente de una clase abstracta**: No puedes hacer `new Animal()`.
- **Se puede crear un objeto de una subclase concreta que extiende de la clase abstracta**: Puedes hacer `new Perro()` si `Perro` extiende `Animal` y proporciona implementaciones para todos los métodos abstractos.
- **Los métodos concretos de la clase abstracta pueden ser usados por las subclases**: Métodos como `respirar`, que tienen una implementación en la clase abstracta `Animal`, pueden ser llamados directamente en las instancias de las subclases.