Sin embargo, puedes implementar la cardinalidad en Java a través de la relación entre clases. Aquí tienes un ejemplo simple de una relación de uno a muchos entre dos clases. Supongamos que tienes una clase `Curso` y una clase `Estudiante`. Cada curso puede tener varios estudiantes, pero un estudiante solo puede pertenecer a un curso a la vez. Esto es una relación de uno a muchos.

------

# CARDINALIDAD 1 - 1

En este ejemplo, tenemos una asociación uno a uno entre las clases `Equipo` y `Jugador`. Esto significa que cada instancia de la clase `Equipo` está asociada con exactamente una instancia de la clase `Jugador`, y viceversa.
```java
class Equipo {
    private String nombre;
    private Jugador jugador;

    public Equipo(String nombre, Jugador jugador) {
        this.nombre = nombre;
        this.jugador = jugador;
    }

    public String getNombre() {
        return nombre;
    }

    public Jugador getJugador() {
        return jugador;
    }
}

class Jugador {
    private String nombre;

    public Jugador(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Jugador jugador = new Jugador("Juan");
        Equipo equipo = new Equipo("Equipo A", jugador);

        System.out.println("Jugador en el equipo " + equipo.getNombre() + ": " + equipo.getJugador().getNombre());
    }
}
```
![[Pasted image 20240318194131.png]]
# CARDINALIDAD 1 - MUCHOS
En una relación de cardinalidad uno a muchos, un objeto de una clase puede estar relacionado con varios objetos de otra clase, pero cada objeto de la segunda clase está relacionado con solo un objeto de la primera clase. Un ejemplo común de esto podría ser una relación entre un profesor y sus estudiantes. Un profesor puede tener muchos estudiantes, pero cada estudiante solo puede tener un profesor. Aquí está el ejemplo:
```java
import java.util.ArrayList;
import java.util.List;

class Profesor {
    private String nombre;
    private List<Estudiante> estudiantes;

    public Profesor(String nombre) {
        this.nombre = nombre;
        this.estudiantes = new ArrayList<>();
    }

    public String getNombre() {
        return nombre;
    }

    public void agregarEstudiante(Estudiante estudiante) {
        estudiantes.add(estudiante);
    }

    public List<Estudiante> getEstudiantes() {
        return estudiantes;
    }
}

class Estudiante {
    private String nombre;
    private Profesor profesor;

    public Estudiante(String nombre, Profesor profesor) {
        this.nombre = nombre;
        this.profesor = profesor;
    }

    public String getNombre() {
        return nombre;
    }

    public Profesor getProfesor() {
        return profesor;
    }
}

public class Main {
    public static void main(String[] args) {
        Profesor profesor = new Profesor("Dr. García");

        Estudiante estudiante1 = new Estudiante("Juan", profesor);
        Estudiante estudiante2 = new Estudiante("María", profesor);

        profesor.agregarEstudiante(estudiante1);
        profesor.agregarEstudiante(estudiante2);

        System.out.println("Estudiantes del profesor " + profesor.getNombre() + ":");
        for (Estudiante estudiante : profesor.getEstudiantes()) {
            System.out.println(estudiante.getNombre());
        }
    }
}
```
