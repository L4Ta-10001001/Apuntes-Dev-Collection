### Relación de Asociación

La relación de asociación en Java se da cuando dos clases están relacionadas entre sí, pero ninguna de las dos tiene una propiedad de pertenencia sobre la otra. Las clases pueden existir independientemente una de la otra, pero colaboran para realizar alguna tarea o compartir información.

**Ejemplo 1: Profesor y Estudiante**

En este ejemplo, un profesor puede enseñar a varios estudiantes, y un estudiante puede tener varios profesores. Ambos pueden existir independientemente uno del otro.

```java
import java.util.ArrayList;
import java.util.List;

class Profesor {
    String nombre;
    List<Estudiante> estudiantes;

    Profesor(String nombre) {
        this.nombre = nombre;
        this.estudiantes = new ArrayList<>();
    }
}

class Estudiante {
    String nombre;
    List<Profesor> profesores;

    Estudiante(String nombre) {
        this.nombre = nombre;
        this.profesores = new ArrayList<>();
    }
}

public class Main {
    public static void main(String[] args) {
        Profesor profesor1 = new Profesor("Eustaquio");
        Profesor profesor2 = new Profesor("Alfonso");

        Estudiante estudiante1 = new Estudiante("Mario");
        Estudiante estudiante2 = new Estudiante("Pepe");

        profesor1.estudiantes.add(estudiante1);
        profesor1.estudiantes.add(estudiante2);

        estudiante1.profesores.add(profesor2);

        System.out.println(profesor1.nombre + " enseña a: ");
        for (Estudiante e : profesor1.estudiantes) {
            System.out.println(e.nombre);
        }

        System.out.println(estudiante1.nombre + " es enseñado por: ");
        for (Profesor p : estudiante1.profesores) {
            System.out.println(p.nombre);
        }
    }
}
```
![[Pasted image 20240520092443.png]]
**Ejemplo 2**
En este ejemplo, un médico puede tener varios pacientes y un paciente puede consultar a varios médicos. Ambos pueden existir independientemente.
```java
class Medico {
    private String nombre;
    private List<Paciente> pacientes;

    public Medico(String nombre) {
        this.nombre = nombre;
        this.pacientes = new ArrayList<>();
    }

    public void agregarPaciente(Paciente paciente) {
        pacientes.add(paciente);
    }

    public String getNombre() {
        return nombre;
    }

    public List<Paciente> getPacientes() {
        return pacientes;
    }
}

class Paciente {
    private String nombre;
    private List<Medico> medicos;

    public Paciente(String nombre) {
        this.nombre = nombre;
        this.medicos = new ArrayList<>();
    }

    public void agregarMedico(Medico medico) {
        medicos.add(medico);
    }

    public String getNombre() {
        return nombre;
    }

    public List<Medico> getMedicos() {
        return medicos;
    }
}

public class Main {
    public static void main(String[] args) {
        Medico medico1 = new Medico("Dr. Smith");
        Medico medico2 = new Medico("Dr. Johnson");

        Paciente paciente1 = new Paciente("Paciente A");
        Paciente paciente2 = new Paciente("Paciente B");

        medico1.agregarPaciente(paciente1);
        medico1.agregarPaciente(paciente2);

        paciente1.agregarMedico(medico2);

        System.out.println(medico1.getNombre() + " tiene a los siguientes pacientes: ");
        for (Paciente p : medico1.getPacientes()) {
            System.out.println(p.getNombre());
        }

        System.out.println(paciente1.getNombre() + " está consultando con los siguientes médicos: ");
        for (Medico m : paciente1.getMedicos()) {
            System.out.println(m.getNombre());
        }
    }
}
```
En ambos ejemplos, las clases `Profesor` y `Estudiante` o `Medico` y `Paciente` están asociadas entre sí, pero ninguna tiene una propiedad de pertenencia fuerte sobre la otra. Ambas pueden existir y funcionar independientemente, aunque colaboren entre ellas.

### Relación de Agregación

En Java, una relación de agregación implica que una clase ("contenedor" o "agregador") tiene una relación de "tiene un" (has-a) con otra clase, pero ambas pueden existir de manera independiente.

En este ejemplo, una biblioteca puede contener múltiples libros, pero los libros pueden existir independientemente de la biblioteca.
```java
import java.util.ArrayList;
import java.util.List;

class Libro {
    private String titulo;

    public Libro(String titulo) {
        this.titulo = titulo;
    }

    public String getTitulo() {
        return titulo;
    }
}

class Biblioteca {
    private String nombre;
    private List<Libro> libros;

    public Biblioteca(String nombre) {
        this.nombre = nombre;
        this.libros = new ArrayList<>();
    }

    public void agregarLibro(Libro libro) {
        libros.add(libro);
    }

    public List<Libro> getLibros() {
        return libros;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Biblioteca biblioteca = new Biblioteca("Biblioteca Central");
        Libro libro1 = new Libro("El Quijote");
        Libro libro2 = new Libro("Cien Años de Soledad");

        biblioteca.agregarLibro(libro1);
        biblioteca.agregarLibro(libro2);

        System.out.println("Libros en la " + biblioteca.getNombre() + ":");
        if (biblioteca.getLibros().size() > 0) System.out.println(biblioteca.getLibros().get(0).getTitulo());
        if (biblioteca.getLibros().size() > 1) System.out.println(biblioteca.getLibros().get(1).getTitulo());
    }
}
```
### Relación de Composición
Es una forma más fuerte de agregación. Aquí, el ciclo de vida de la clase contenida está estrechamente ligado a la clase contenedora. Si la clase contenedora es destruida, también lo es la clase contenida.
```java
class Engine {
    void start() {
        System.out.println("Engine started.");
    }
}

class Car {
    private final Engine engine; // Composición

    Car() {
        engine = new Engine();
    }

    void startCar() {
        engine.start();
        System.out.println("Car started.");
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.startCar();
    }
}
```
