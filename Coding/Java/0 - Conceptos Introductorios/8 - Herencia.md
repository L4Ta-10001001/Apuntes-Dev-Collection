Permite a una clase (llamada clase hija o subclase) heredar atributos y métodos de otra clase (llamada clase padre o superclase), lo que facilita la reutilización de código y la organización jerárquica de las clases.

Cuando una clase hereda de otra, la clase hija adquiere todas las propiedades (métodos y atributos) de la clase padre y puede añadir sus propios métodos o atributos adicionales, o incluso sobrescribir los métodos de la clase padre para adaptarlos a sus necesidades específicas.

La herencia permite la creación de una jerarquía de clases, donde las clases más específicas (hijas) pueden heredar comportamientos y características de las clases más generales (padres), lo que promueve la modularidad, la cohesión y la facilidad de mantenimiento del código.

---------

En este ejemplo, la clase `Animal` es la clase padre o superclase, que define un método `hacerSonido`. La clase `Perro` es la clase hija o subclase que hereda de la clase `Animal`. La clase `Perro` sobrescribe el método `hacerSonido` para representar el sonido específico de un perro. En el método `main`, creamos una instancia de la clase `Perro` y llamamos al método `hacerSonido` para imprimir el sonido del perro.
```java
class Animal {
    // Campo de la clase Animal
    protected String color;

    // Método de la clase Animal
    public void hacerSonido() {
        System.out.println("El animal hace un sonido genérico.");
    }
}

class Perro extends Animal {
    // Método de la clase Perro que sobrescribe el método de la clase Animal
    @Override
    public void hacerSonido() {
        System.out.println("El perro dice ¡Guau! ¡Guau! y es de color " + color); 
    }

}

public class Main {
    public static void main(String[] args) {
        Perro miPerro = new Perro();

        // Accediendo al campo tipo heredado de la clase Animal
        miPerro.color = "Marrón";

        // Accediendo al método hacerSonido() heredado de la clase Animal
        miPerro.hacerSonido();

    }
}
```
También se pueden heredar métodos, por ejemplo un método llamado ladrar:
```java
class Animal {
    protected String color;

    public void hacerSonido() {
        System.out.println("El animal hace un sonido genérico.");
    }

    public void ladrar() {
        System.out.println("El animal ladra y es de color " + color);
    }

}

class Perro extends Animal {
    @Override
    public void hacerSonido() {
        System.out.println("El perro dice ¡Guau! ¡Guau! y es de color " + color);
    }
}

public class Main {
    public static void main(String[] args) {
        Perro miPerro = new Perro();

        miPerro.color = "Marrón";

        miPerro.ladrar(); // Aquí se hereda el método ladrar

    }
}
```
![[Pasted image 20240701101043.png]]
## EXTENDS
La palabra clave `extends` se utiliza para establecer una relación de herencia entre dos clases:
![[Pasted image 20240318201733.png]]
Este sería el resultado:
![[Pasted image 20240318201750.png]]
## ANOTACIÓN @OVERRIDE
  
La anotación `@Override` en Java se utiliza para indicar que un método en una subclase está sobrescribiendo un método de la superclase. Esto ayuda a mejorar la legibilidad y claridad del código.

---------

En este ejemplo, la clase `Perro` hereda de la clase `Animal` y sobrescribe el método `comer()` para personalizar el comportamiento para los perros. La anotación `@Override` se utiliza antes del método `comer()` en la clase `Perro`, lo que indica que estamos sobrescribiendo el método `comer()` de la clase padre `Animal`.
```java
class Animal {
    public void comer() {
        System.out.println("El animal está comiendo...");
    }
}

class Perro extends Animal {
    @Override
    public void comer() {
        System.out.println("El perro está comiendo croquetas.");
    }
}

public class Main {
    public static void main(String[] args) {
        Perro miPerro = new Perro();
        miPerro.comer(); // Llama al método comer() de la subclase Perro
    }
}
```
![[Pasted image 20240318201043.png]]
## PROTECTED
`protected`, significa que ese miembro es accesible dentro del mismo paquete y también por todas las subclases:
![[Pasted image 20240318201945.png]]
Por lo tanto, `protected` proporciona un nivel de encapsulación que permite un cierto grado de acceso a los miembros de la clase desde las subclases, mientras que aún se mantiene la encapsulación y la seguridad de los datos dentro del objeto.

-------------

### Relaciones de clases asociacion, agregacion debil, composicion y generalizacion

**Agregación**
En la agregación, una clase contiene una referencia a otra clase, pero la segunda clase no es parte esencial de la primera. Por ejemplo, un "Departamento" puede tener "Empleados". La clase "Departamento" puede contener una lista de objetos "Empleado", pero los empleados pueden existir sin depender completamente del departamento.
```java
import java.util.List;
import java.util.ArrayList;

class Departamento {
    private String nombre;
    private List<Empleado> empleados;

    public Departamento(String nombre) {
        this.nombre = nombre;
        this.empleados = new ArrayList<>();
    }

    public void agregarEmpleado(Empleado empleado) {
        empleados.add(empleado);
    }

    // Otros métodos y getters/setters
}

class Empleado {
    private String nombre;
    private int edad;

    public Empleado(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    // Otros métodos y getters/setters
}

public class Main {
    public static void main(String[] args) {
        Departamento departamento = new Departamento("Ventas");
        Empleado emp1 = new Empleado("Juan", 30);
        departamento.agregarEmpleado(emp1);

        // Otros procesos
    }
}
```
**Composición**: Similar a la agregación, pero en este caso, la segunda clase es parte integral de la primera. Esto significa que la segunda clase no puede existir independientemente de la primera. Por ejemplo, un "Auto" puede tener "Ruedas". Las ruedas están completamente asociadas con el auto y no pueden existir sin él.
```java
class Auto {
    private String marca;
    private Rueda[] ruedas;

    public Auto(String marca) {
        this.marca = marca;
        this.ruedas = new Rueda[4];
        for (int i = 0; i < 4; i++) {
            ruedas[i] = new Rueda("Michelin", 17.5); // Ejemplo de marca y diámetro
        }
    }

    public void conducir() {
        System.out.println("El auto de marca " + marca + " está en movimiento.");
    }

    // Getters y setters
    public String getMarca() {
        return marca;
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }

    public Rueda[] getRuedas() {
        return ruedas;
    }

    public void setRuedas(Rueda[] ruedas) {
        this.ruedas = ruedas;
    }
}

class Rueda {
    private String marca;
    private double diametro;

    public Rueda(String marca, double diametro) {
        this.marca = marca;
        this.diametro = diametro;
    }

    public void girar() {
        System.out.println("La rueda de marca " + marca + " está girando.");
    }

    // Getters y setters
    public String getMarca() {
        return marca;
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }

    public double getDiametro() {
        return diametro;
    }

    public void setDiametro(double diametro) {
        this.diametro = diametro;
    }
}

public class Main {
    public static void main(String[] args) {
        Auto auto = new Auto("Toyota");
        auto.conducir(); // Ejemplo de método del auto

        // Acceder a las ruedas del auto
        for (Rueda rueda : auto.getRuedas()) {
            rueda.girar(); // Ejemplo de método de la rueda
        }

        // Otros procesos
    }
}
```
**Asociación simple**: Esto ocurre cuando una clase utiliza los servicios de otra clase, pero no hay dependencia fuerte entre ellas. Por ejemplo, una clase "Orden" puede estar asociada con una clase "Cliente". Cada orden puede estar vinculada a un cliente, pero los clientes y las órdenes existen de manera independiente.
```java
class Orden {
    private int numero;
    private Cliente cliente;

    public Orden(int numero, Cliente cliente) {
        this.numero = numero;
        this.cliente = cliente;
    }

    // Getter y setter para el número de orden
    public int getNumero() {
        return numero;
    }

    public void setNumero(int numero) {
        this.numero = numero;
    }

    // Getter y setter para el cliente
    public Cliente getCliente() {
        return cliente;
    }

    public void setCliente(Cliente cliente) {
        this.cliente = cliente;
    }

    // Otros métodos si son necesarios
}

class Cliente {
    private String nombre;
    private String direccion;

    public Cliente(String nombre, String direccion) {
        this.nombre = nombre;
        this.direccion = direccion;
    }

    // Getter y setter para el nombre del cliente
    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    // Getter y setter para la dirección del cliente
    public String getDireccion() {
        return direccion;
    }

    public void setDireccion(String direccion) {
        this.direccion = direccion;
    }

    // Otros métodos si son necesarios
}

public class Main {
    public static void main(String[] args) {
        Cliente cliente = new Cliente("Juan", "Calle Principal");
        Orden orden = new Orden(1001, cliente);

        // Ejemplo de cómo obtener información de la orden y del cliente
        System.out.println("Número de orden: " + orden.getNumero());
        System.out.println("Nombre del cliente: " + orden.getCliente().getNombre());
        System.out.println("Dirección del cliente: " + orden.getCliente().getDireccion());
    }
}
```


