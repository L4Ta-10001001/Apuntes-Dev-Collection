### MÉTODO GETTERS
Un getter sirve para acceder a un atributo privado de una clase desde fuera de esa clase. En otras palabras, un getter proporciona un medio seguro y controlado para obtener el valor de un atributo privado.
```java
public class Persona {
    // Atributos
    private String nombre;
    private int edad;
    
    // Constructor
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    
    // Getter para obtener el nombre
    public String getNombre() {
        return nombre;
    }
    
    // Getter para obtener la edad
    public int getEdad() {
        return edad;
    }
    
    public static void main(String[] args) {
        // Crear un objeto Persona
        Persona persona = new Persona("Juan", 25);
        
        // Usar getters para obtener información
        System.out.println("Nombre: " + persona.getNombre());
        System.out.println("Edad: " + persona.getEdad());
    }
}
```
### MÉTODO SETTERS
Un setter sirve para establecer el valor de un atributo privado de una clase desde fuera de esa clase. En otras palabras, un setter proporciona un método controlado y seguro para modificar el valor de un atributo privado.

```java
public class Persona {
    // Atributos
    private String nombre;
    private int edad;
    
    // Constructor
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    
    // Getter para obtener el nombre
    public String getNombre() {
        return nombre;
    }
    
    // Setter para establecer el nombre
    public void setNombre(String nombre) {
        this.nombre = nombre;
    }
    
    // Getter para obtener la edad
    public int getEdad() {
        return edad;
    }
    
    public static void main(String[] args) {
        // Crear un objeto Persona
        Persona persona = new Persona("Juan", 25);
        
        // Usar getters para obtener información
        System.out.println("Nombre: " + persona.getNombre());
        System.out.println("Edad: " + persona.getEdad());
        
        // Usar el setter para cambiar el nombre
        persona.setNombre("Pedro");
        
        // Usar el getter para obtener el nombre actualizado
        System.out.println("Nombre actualizado: " + persona.getNombre());
    }
}
```

Aquí hay algunas razones por las cuales se utilizan los setters:

1. **Encapsulamiento**: Ayudan a implementar el principio de encapsulamiento en la programación orientada a objetos. Al hacer que los atributos sean privados y proporcionar setters, se controla el acceso y la modificación de esos atributos. Esto significa que se pueden aplicar lógicas adicionales o restricciones antes de modificar el valor del atributo.
    
2. **Validación de datos**: Los setters pueden incluir lógica adicional para validar los datos antes de establecerlos. Por ejemplo, podrían verificar que un valor esté dentro de un rango específico o cumplir ciertas condiciones antes de permitir la modificación.
    
3. **Control de acceso**: Los setters permiten controlar y restringir cómo se modifican los datos de un objeto. Esto es útil para mantener la integridad de los datos y prevenir situaciones no deseadas.
    
4. **Flexibilidad**: Si necesitas realizar cambios en la forma en que se establece un atributo en el futuro, puedes hacerlo fácilmente actualizando el código dentro del setter, manteniendo al mismo tiempo el contrato de cómo se accede y se establece el atributo.
    
5. **Mantenimiento del código**: Al utilizar setters en lugar de permitir el acceso directo a los atributos, se reduce la cantidad de lugares en el código donde se debe hacer cambios si alguna lógica relacionada con la modificación de los datos cambia en el futuro.