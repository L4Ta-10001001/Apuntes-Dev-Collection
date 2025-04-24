Un interbloqueo, también conocido como "deadlock" en inglés, es una situación en la programación concurrente donde dos o más hilos (threads) están bloqueados de forma indefinida, esperando que un recurso que nunca será liberado. Esto sucede cuando cada hilo está esperando por un recurso que está retenido por otro hilo, creando un ciclo de dependencia que impide que cualquiera de los hilos continúe su ejecución.

En este caso, utilizaremos dos objetos de bloqueo y dos hilos. Cada hilo intentará adquirir ambos bloqueos en orden inverso, lo que puede causar un interbloqueo.
```java
public class SimpleDeadlock {
    // Dos objetos de bloqueo
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();

    public void method1() {
        synchronized (lock1) {
            System.out.println("Thread 1: Acquired lock1, waiting for lock2...");
            try { Thread.sleep(100); } catch (InterruptedException e) {}

            synchronized (lock2) {
                System.out.println("Thread 1: Acquired lock2");
            }
        }
    }

    public void method2() {
        synchronized (lock2) {
            System.out.println("Thread 2: Acquired lock2, waiting for lock1...");
            try { Thread.sleep(100); } catch (InterruptedException e) {}

            synchronized (lock1) {
                System.out.println("Thread 2: Acquired lock1");
            }
        }
    }

    public static void main(String[] args) {
        SimpleDeadlock deadlock = new SimpleDeadlock();

        // Crear dos hilos que ejecutarán method1 y method2
        Thread thread1 = new Thread(new Runnable() {
            public void run() {
                deadlock.method1();
            }
        });

        Thread thread2 = new Thread(new Runnable() {
            public void run() {
                deadlock.method2();
            }
        });

        // Iniciar los hilos
        thread1.start();
        thread2.start();
    }
}
```

### Explicación Paso a Paso

1. **Definición de Bloqueos**: Tenemos dos objetos de bloqueo `lock1` y `lock2`.
    
2. **Método `method1`**:
    
    - El hilo `Thread1` adquiere `lock1`.
    - Luego, `Thread1` duerme por 100 milisegundos.
    - Después de dormir, `Thread1` intenta adquirir `lock2`.
3. **Método `method2`**:
    
    - El hilo `Thread2` adquiere `lock2`.
    - Luego, `Thread2` duerme por 100 milisegundos.
    - Después de dormir, `Thread2` intenta adquirir `lock1`.
4. **Creación y Ejecución de Hilos**:
    
    - Se crean `thread1` y `thread2`.
    - `thread1` ejecuta `method1`.
    - `thread2` ejecuta `method2`.
    - Ambos hilos se inician casi al mismo tiempo.

### Interbloqueo

En este ejemplo:

- `Thread1` adquiere `lock1` y duerme.
- `Thread2` adquiere `lock2` y duerme.
- Cuando `Thread1` se despierta, intenta adquirir `lock2`, pero `Thread2` ya lo tiene.
- Cuando `Thread2` se despierta, intenta adquirir `lock1`, pero `Thread1` ya lo tiene.
- Ninguno de los hilos puede continuar porque están esperando indefinidamente a que el otro libere el bloqueo que necesita, lo que causa un interbloqueo.

Este ejemplo muestra claramente cómo puede ocurrir un interbloqueo cuando dos hilos intentan adquirir recursos en orden inverso, y es una buena base para entender y evitar interbloqueos en programas más complejos.