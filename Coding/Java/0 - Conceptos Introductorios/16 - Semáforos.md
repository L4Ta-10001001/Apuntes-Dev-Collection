El propósito del semáforo en este código es controlar el acceso concurrente a un recurso compartido (representado por la clase `SharedResource`). Los semáforos permiten limitar el número de threads que pueden acceder al recurso simultáneamente. En este caso, el semáforo permite que solo dos threads accedan al recurso al mismo tiempo.

------

### Explicación del código:

1. **Clase `SharedResource`**: Esta clase representa el recurso compartido. En este ejemplo, no tiene contenido, pero en un caso real contendría los datos o métodos a los que los threads acceden.
    
2. **Clase `Worker`**: Esta clase implementa la interfaz `Runnable` y representa un trabajador (o thread) que intentará acceder al recurso compartido.
    
    - El constructor `Worker` toma un semáforo y el nombre del trabajador.
    - En el método `run`, el trabajador intenta adquirir un permiso del semáforo utilizando `semaphore.acquire()`. Si se obtiene un permiso, el thread puede acceder al recurso compartido.
    - Después de simular el acceso al recurso (usando `Thread.sleep(2000)` para esperar 2 segundos), el permiso se libera con `semaphore.release()`.
3. **Clase `SemaphoreExample`**: Esta es la clase principal que crea un semáforo con 2 permisos y lanza varios threads de `Worker`.


Imaginemos un escenario donde varios trabajadores (`Worker`) necesitan acceder a un recurso compartido (`SharedResource`). El recurso compartido podría ser cualquier cosa, como una base de datos, un archivo o una impresora. La clase `SharedResource` tiene un método que simula el trabajo realizado por los trabajadores, por ejemplo, procesar datos. En nuestro ejemplo, cuando un trabajador accede al recurso, se imprime un mensaje indicando que el trabajador está trabajando en el recurso, seguido de una pausa para simular el tiempo de trabajo, y finalmente se imprime un mensaje indicando que el trabajador ha terminado.

Cada trabajador se representa mediante una clase `Worker` que extiende `Thread`. En su método `run`, el trabajador primero intenta adquirir un permiso del semáforo. Si obtiene un permiso, el trabajador procede a acceder al recurso compartido y realiza su tarea. Después de completar la tarea, el trabajador libera el permiso, permitiendo que otros trabajadores puedan adquirirlo y acceder al recurso. Si no hay permisos disponibles, el trabajador se bloquea y espera hasta que un permiso sea liberado.

La clase principal del programa, digamos `SemaphoreExample`, se encarga de crear el semáforo y los trabajadores. Un semáforo con un contador inicial de, por ejemplo, dos, permite que un máximo de dos trabajadores accedan al recurso compartido al mismo tiempo. La clase principal también crea una instancia del recurso compartido y lanza varios threads de trabajadores. Cada trabajador tiene acceso al mismo semáforo y recurso compartido.

El flujo del programa es sencillo: cada trabajador intenta adquirir un permiso del semáforo antes de acceder al recurso compartido. Si el semáforo tiene permisos disponibles, el trabajador obtiene un permiso y accede al recurso. Después de completar su tarea, libera el permiso. Si no hay permisos disponibles, el trabajador que intenta adquirir un permiso se bloquea hasta que otro trabajador libere un permiso.

La principal ventaja del uso de semáforos es el control preciso sobre el acceso a recursos compartidos, evitando problemas de concurrencia como condiciones de carrera y proporcionando una forma eficiente de gestionar múltiples threads que necesitan acceso a recursos limitados. Los semáforos aseguran que no más de un número específico de threads accedan simultáneamente al recurso, manteniendo así la integridad y consistencia del recurso.

----------

Este ejemplo muestra cómo se puede utilizar un semáforo para controlar el acceso a un recurso compartido, asegurando que no más de dos threads puedan acceder al mismo tiempo.

```java
import java.util.concurrent.Semaphore;

class SharedResource {
    // Método que simula una operación en el recurso compartido
    public void doWork(String workerName) {
        System.out.println(workerName + " está trabajando en el recurso compartido.");
        try {
            // Simula tiempo de trabajo
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(workerName + " ha terminado de trabajar en el recurso compartido.");
    }
}

class Worker extends Thread {
    private Semaphore semaphore;
    private SharedResource sharedResource;
    private String workerName;

    public Worker(Semaphore semaphore, SharedResource sharedResource, String workerName) {
        this.semaphore = semaphore;
        this.sharedResource = sharedResource;
        this.workerName = workerName;
    }

    @Override
    public void run() {
        try {
            System.out.println(workerName + " está esperando un permiso.");
            semaphore.acquire();
            System.out.println(workerName + " ha obtenido un permiso.");

            // Acceder al recurso compartido
            sharedResource.doWork(workerName);

            System.out.println(workerName + " ha liberado el permiso.");
            semaphore.release();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class SemaphoreExample {
    public static void main(String[] args) {
        // Crear un semáforo con 2 permisos.
        Semaphore semaphore = new Semaphore(2);
        SharedResource sharedResource = new SharedResource();

        // Crear y lanzar múltiples threads.
        Worker t1 = new Worker(semaphore, sharedResource, "Worker 1");
        Worker t2 = new Worker(semaphore, sharedResource, "Worker 2");
        Worker t3 = new Worker(semaphore, sharedResource, "Worker 3");
        Worker t4 = new Worker(semaphore, sharedResource, "Worker 4");

        t1.start();
        t2.start();
        t3.start();
        t4.start();
    }
}
```
![[Pasted image 20240617110025.png]]

