En Java, los modificadores de acceso son palabras clave que se utilizan para controlar el acceso a las clases, variables, métodos y constructores. Hay cuatro tipos de modificadores de acceso en Java:

1. **public:** Los miembros (clases, variables, métodos y constructores) marcados como públicos son accesibles desde cualquier clase en el mismo paquete o desde cualquier otra clase en un paquete diferente. Por ejemplo:
```java
public class Main {
    public static void main(String[] args) {
        Persona persona = new Persona("Juan");

        // Intento de acceso directo al campo privado (esto causará un error de compilación)
        System.out.println(persona.nombre);
    }
}

class Persona {
    public String nombre; // El modificador de acceso es público, por lo que se puede acceder desde otra clase.

    // Constructor
    public Persona(String nombre) {
        this.nombre = nombre;
    }

    // Método para obtener el nombre
    public String getNombre() {
        return nombre;
    }

    // Método para establecer el nombre
    public void setNombre(String nombre) {
        this.nombre = nombre;
    }
}
```
![[Pasted image 20240613110654.png]]
2. **private:** Los miembros marcados como privados solo son accesibles dentro de la misma clase. No son accesibles desde clases fuera de la clase en la que están declarados. Por ejemplo:
```java
public class Main {
    public static void main(String[] args) {
        Persona persona = new Persona("Juan");

        // Intento de acceso directo al campo privado (esto causará un error de compilación)
        System.out.println(persona.nombre);
    }
}

class Persona {
    private String nombre;

    // Constructor
    public Persona(String nombre) {
        this.nombre = nombre;
    }

    // Método para obtener el nombre
    public String getNombre() {
        return nombre;
    }

    // Método para establecer el nombre
    public void setNombre(String nombre) {
        this.nombre = nombre;
    }
}
```
Este código dará error porque un identificador de acceso private en una clase de Java no puede ser accedido directamente desde otra clase.
![[Pasted image 20240613110536.png]]

3. **protected:** Los miembros marcados como protegidos son accesibles dentro de la misma clase, en las clases del mismo paquete y en las subclases (independientemente del paquete). Por ejemplo:
```java
public class MyClass {
    protected int myVariable;
    protected void myMethod() {
        // código del método
    }
}
```
4. **default (sin especificar):** Si no se especifica ningún modificador de acceso, el miembro es de acceso predeterminado (también conocido como "paquete"). Los miembros predeterminados son accesibles solo dentro del mismo paquete. Por ejemplo:
```java
class MyClass {
    int myVariable;
    void myMethod() {
        // código del método
    }
}
```