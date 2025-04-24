La API de Java son las bibliotecas de clases que vienen predefinidas con el lenguaje java para que las podamos utilizar. 
![[Pasted image 20240307120057.png]]
### 2 Tipos de Clases en Java

**Predefinidas**
Vienen incluidas con el lenguaje (dentro de la API de java)

- Incluyen clases fundamentales como `String`, `Integer`, `Double`, `ArrayList`, entre otras.
- Son proporcionadas por Java y se pueden utilizar sin necesidad de definirlas explícitamente.
- Estas clases están diseñadas para realizar tareas comunes y proporcionar funcionalidades esenciales.

**Propias**

- Estas son clases que los programadores crean según las necesidades específicas de su aplicación.
- Permiten la abstracción y encapsulación de datos y comportamientos relacionados en una única unidad.
- Pueden contener atributos (variables) y métodos (funciones) definidos por el programador.
- Son instanciadas para crear objetos que representan conceptos del dominio de la aplicación.

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

    // Método
    public void saludar() {
        System.out.println("Hola, soy " + nombre + " y tengo " + edad + " años.");
    }
}

// Uso de la clase propia
Persona persona1 = new Persona("Juan", 25);
persona1.saludar();
```
### PAQUETES EN JAVA
  
En Java, un paquete es un mecanismo de organización de clases y interfaces relacionadas en un espacio de nombres común. Los paquetes sirven para evitar conflictos de nombres y proporcionar una estructura modular y organizada al código fuente. Aquí hay una explicación más detallada de los paquetes en Java:

1. **Organización del código:** Los paquetes ayudan a organizar el código fuente de un programa en módulos lógicos. Esto facilita la gestión y el mantenimiento del código, especialmente en proyectos grandes.
2. **Paquetes estándar de Java:** Java tiene una serie de paquetes estándar (por ejemplo, `java.lang`, `java.util`, `java.io`) que contienen clases y utilidades esenciales. Estos paquetes están disponibles de forma predeterminada y no requieren declaraciones de importación.

Dentro de la web api java, tenemos un apartado para los paquetes:
![[Pasted image 20240307115425.png]]
Donde por ejemplo nos encontramos con clases que ya hemos utilizado anteriormente como la clase Math:
![[Pasted image 20240307115923.png]]
**Creación de paquetes:** Para organizar clases en paquetes, simplemente coloca tus archivos fuente en directorios que reflejen la estructura de paquetes deseada. La primera línea de un archivo fuente Java debe especificar el paquete al que pertenece la clase:
```java
package mi.paquete;

public class MiClase {
    // ...
}
```
## PAQUETE JAVA.LANG Y CÓMO USAR OTROS
Por defecto, en java se utiliza el paquete java.lang y por tanto las clases de este paquete se pueden usar sin problema, como hicimos anteriormente usando las clases string o math. Pero si queremos usar clases de otro paquete, tenemos que indicarlo específicamente:
![[Pasted image 20240307120329.png]]
Por ejemplo, si intentamos usar la clase scanner del paquete Java.util, veremos que por defecto no podemos:
![[Pasted image 20240307120804.png]]
Vemos que no podemos utilizarla porque a diferencia de la clase string o math que pertenecen a java.lang, en este caso al pertenecer a otra clase no las podemos usar:
![[Pasted image 20240307120910.png]]
Pero si lo indicamos al principio del todo con un import, podemos poner que queremos incluir todas las clases del paquete correspondiente, que en este caso la clase scanner pertenece a la clase java.util:
```java
import java.util.*;

public class codigo {

    public static void main(String[] args) {
                
        String nombre;
        Scanner miobjeto;

    }
}
```
Donde vemos que ya no nos sale el mismo error del principio y ahora sí podemos usar esta clase:
![[Pasted image 20240307121142.png]]
O también podemos hacer que nos importe sólo la clase que queramos utilizar:
![[Pasted image 20240307121240.png]]
