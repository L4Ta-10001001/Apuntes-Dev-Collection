**HILO** -> Un hilo es una unidad de computación que se ejecuta dentro del contexto de un proceso. Es decir, se podría considerar un hilo como un subproceso, por ejemplo si abrimos 5 pestañas del navegador, estaríamos corriendo 5 hilos del mismo proceso.

Con los mismos recursos que utiliza el proceso, se lo va repartiendo por los distintos hilos.

**MULTIHILO** -> Un proceso trabaja con múltiples hilos. 

En este punto, hay que ver que los hilos se pueden ejecutar de forma asíncrona (todos los hilos se ejecutan a la ves) o secuencial (termina un hilo, y comienza otro)

En Java, hay dos formas principales de crear un hilo:

1. **Implementando la interfaz `Runnable`**
2. **Extendiendo la clase `Thread`**
### Extender Clase Thread (Concurrente)
Otra forma de crear un hilo es extendiendo la clase `Thread` y sobrescribiendo el método `run()`.
```java
class MyThread extends Thread {
    private String threadName;

    public MyThread(String name) {
        this.threadName = name;
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Hilo de " + threadName + ": " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread("Thread 1");
        MyThread thread2 = new MyThread("Thread 2");
        MyThread thread3 = new MyThread("Thread 3");

        thread1.start(); // Inicia el hilo 1
        thread2.start(); // Inicia el hilo 2
        thread3.start(); // Inicia el hilo 3
    }
}
```
![[Pasted image 20240613102820.png]]
- **Múltiples hilos:** Se crean y se inician tres hilos (`thread1`, `thread2`, `thread3`).
- **Ejecución independiente:** Cada hilo ejecuta su propio método `run` de manera independiente, imprimiendo mensajes y durmiendo por 1 segundo en cada iteración.
- **Intercalación:** La salida de los mensajes de los hilos se intercalará debido a la planificación de la CPU, lo que es una característica de la concurrencia.
#### SINCRONIZAR HILOS
En Java, la sincronización de hilos es crucial para asegurar que múltiples hilos puedan interactuar correctamente sin interferir entre sí cuando acceden a recursos compartidos como variables o métodos.

- **Consistencia de Datos**: Cuando varios hilos acceden y modifican variables compartidas, la sincronización garantiza que los cambios sean visibles para todos los hilos de manera consistente y en el orden correcto.
    
- **Evitar Condiciones de Carrera**: Una condición de carrera ocurre cuando el resultado de la ejecución depende del orden de ejecución de los hilos. La sincronización ayuda a evitar este problema asegurando que solo un hilo a la vez pueda acceder a ciertas secciones críticas del código.
    
- **Bloqueo de Recursos Compartidos**: Al utilizar métodos o bloques sincronizados, se puede bloquear el acceso a recursos compartidos hasta que un hilo termine de usarlos, previniendo así que otros hilos interfieran en el proceso.

Podemos añadir sincronización para evitar condiciones de carrera y conseguir que los hilos se vayan ejecutando por orden:
```java
class MyThread extends Thread {
    private String threadName;

    public MyThread(String name) {
        this.threadName = name;
    }

    // Método run sincronizado
    @Override
    public synchronized void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Hilo de " + threadName + ": " + i);
            try {
                Thread.sleep(1000); // Pausa el hilo por 1 segundo
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread("Thread 1");
        MyThread thread2 = new MyThread("Thread 2");
        MyThread thread3 = new MyThread("Thread 3");

        thread1.start(); // Inicia el hilo 1
        thread2.start(); // Inicia el hilo 2
        thread3.start(); // Inicia el hilo 3
    }
}
```
![[Pasted image 20240624153107.png]]
O también podemos sincronizarlo de la siguiente forma, donde añadimos synchronized (Main.class) justo antes del punto crítico:
```java
class MyThread extends Thread {
    private String threadName;

    public MyThread(String name) {
        this.threadName = name;
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            synchronized (Main.class) {
                System.out.println("Hilo de " + threadName + ": " + i);
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread("Thread 1");
        MyThread thread2 = new MyThread("Thread 2");
        MyThread thread3 = new MyThread("Thread 3");

        thread1.start(); // Inicia el hilo 1
        thread2.start(); // Inicia el hilo 2
        thread3.start(); // Inicia el hilo 3
    }
}
```

### Ejemplo de condición de carrera

```java
import java.util.Random;

public class Main {
    private static int contador = 0;
    private static Random random = new Random();

    public static void main(String[] args) {
        Runnable tarea = () -> {
            for (int i = 0; i < 1000; i++) {
                int incremento = contador; // Lectura inicial del contador

                // Introducir una pequeña variabilidad en el tiempo de ejecución
                try {
                    Thread.sleep(random.nextInt(5)); // Espera aleatoria de hasta 5 milisegundos
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                contador = incremento + 1; // Incremento del contador
            }
        };

        Thread hilo1 = new Thread(tarea);
        Thread hilo2 = new Thread(tarea);

        hilo1.start();
        hilo2.start();

        try {
            hilo1.join();
            hilo2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Valor final del contador: " + contador);
    }
}
```
En este código, el resultado que debería dar sería 2000, pero debido a la condición de carrera, unas veces dará 1000 y otras otro dato:
![[Pasted image 20240627213926.png]]
Si queremos que los hilos estén sincronizados, añadimos synchronized en la sección crítica que es después del bucle for donde se produce la condición de carrera y se lee el contador:
```java
import java.util.Random;

public class Main {
    private static int contador = 0;
    private static Random random = new Random();

    public static void main(String[] args) {
        Runnable tarea = () -> {
            for (int i = 0; i < 1000; i++) {
                // Sincronizar el bloque crítico con synchronized
                synchronized (Main.class) {
                    int incremento = contador; // Lectura inicial del contador

                    // Introducir una pequeña variabilidad en el tiempo de ejecución
                    try {
                        Thread.sleep(random.nextInt(5)); // Espera aleatoria de hasta 5 milisegundos
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    contador = incremento + 1; // Incremento del contador
                }
            }
        };

        Thread hilo1 = new Thread(tarea);
        Thread hilo2 = new Thread(tarea);

        hilo1.start();
        hilo2.start();

        try {
            hilo1.join();
            hilo2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Valor final del contador: " + contador);
    }
}
```
![[Pasted image 20240627214048.png]]
El bloque synchronized debemos añadirlo en aquellas partes del código donde se acceden y modifican variables compartidas entre múltiples hilos, por ejemplo aquí:
![[Pasted image 20240627214314.png]]
O aquí:
![[Pasted image 20240627214345.png]]
Main.class es una referencia al objeto `Class` de la clase `Main` en tiempo de ejecución. Se utiliza principalmente para:

- Sincronización de bloques de código críticos a nivel de clase.
- Obtener metadatos sobre la clase `Main` en tiempo de ejecución.
- Acceder a recursos estáticos o métodos estáticos de la clase.
- En el caso de `Main.class`, se refiere a la línea donde se declara la clase `Main`, típicamente al inicio del archivo Java


##### Método Join
Su función principal es coordinar la ejecución de múltiples hilos, permitiendo que un hilo espere a que otro termine antes de continuar con su ejecución. A continuación se muestra un ejemplo de uso:
```java
public class JoinExample {
    public static void main(String[] args) {
        Task thread1 = new Task("Thread-1");
        Task thread2 = new Task("Thread-2");

        thread1.start();
        thread2.start();

        try {
            System.out.println("Esperando que termine Thread-1");
            thread1.join(); // El hilo principal espera a que termine thread1
            System.out.println("Thread-1 ha terminado");

            System.out.println("Esperando que termine Thread-2");
            thread2.join(); // El hilo principal espera a que termine thread2
            System.out.println("Thread-2 ha terminado");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Todos los hilos han terminado");
    }
}

class Task extends Thread {
    public Task(String name) {
        super(name);
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " está corriendo");
        try {
            Thread.sleep(2000); // Simulando una tarea que toma tiempo
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName() + " ha terminado");
    }
}
```
### DIFERENCIA ENTRE CONCURRENCIA Y PARALELISMO
### Concurrencia

1. **Definición**:
    
    - La concurrencia se refiere a la capacidad de un sistema para gestionar múltiples tareas al mismo tiempo. No implica necesariamente que estas tareas se estén ejecutando simultáneamente, sino que están progresando en el tiempo de forma intercalada.

![[Pasted image 20240613100726.png]]

### Paralelismo

1. **Definición**:
    
    - El paralelismo se refiere a la capacidad de un sistema para ejecutar múltiples tareas exactamente al mismo tiempo. Esto requiere múltiples núcleos de CPU o múltiples máquinas.

![[Pasted image 20240613100741.png]]

#### Resumen de Diferencias

- **Concurrencia**:
    
    - **Objetivo**: Gestionar múltiples tareas de forma que parezcan estar ejecutándose simultáneamente.
    - **Requisitos**: Puede funcionar en sistemas de un solo núcleo cambiando rápidamente entre tareas.
    - **Ejemplo**: Multitarea en sistemas operativos, donde múltiples aplicaciones parecen ejecutarse al mismo tiempo.
    
- **Paralelismo**:
    
    - **Objetivo**: Ejecutar múltiples tareas al mismo tiempo.
    - **Requisitos**: Requiere múltiples núcleos de CPU o múltiples máquinas.
    - **Ejemplo**: Procesamiento paralelo en sistemas multicore, donde cada núcleo ejecuta una tarea diferente simultáneamente.

La concurrencia se refiere a la estructura y el diseño del software que permite que múltiples tareas progresen de manera intercalada, mientras que el paralelismo se refiere a la ejecución simultánea de múltiples tareas en hardware de múltiples núcleos.
