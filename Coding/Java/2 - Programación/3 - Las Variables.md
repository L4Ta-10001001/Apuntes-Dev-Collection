
Una variable es un espacio en la memoria del ordenador donde se almacenará un valor que podrá cambiar durante la ejecución del programa.

A la hora de crear una variable, tenemos que especificar el tipo de dato que almacenará en su interior + el nombre de la variable. Por ejemplo: int salario o String nombre
### Variable Tipo String
Si quiero crear una variable de tipo string, debo indicarlo al principio, de la siguiente forma:
```java
public class Main {

    public static void main(String[] args) {
                
        String myString = "Esto es una variable"; # Aquí creamos la variable.
        myString = "Aquí cambio el valor de la cadena de texto"; # Aquí la podemos modificar.
        System.out.println(myString);

    }
}
```
Y vemos que imprimimos el valor de la variable y además que la podemos modificar:
![[Pasted image 20240304170123.png]]
### CONCATENAR DATOS
Podemos concatenar un string con la impresión de una variable con el signo de +:
```java
public class Main {

    public static void main(String[] args) {
                
        String myString = "VARIABLE";
        System.out.println("Este es el contenido de la variable: " + myString);

    }
}
```
![[Pasted image 20240304194356.png]]
Vamos a ver otro ejemplo de concatenación de datos:
```java
public class codigo {

    public static void main(String[] args) {
                
        String nombre ="Mario";
        int edad =28;
        double altura=180;
        System.out.println("Mi nombre es " + nombre + " y mi edad " + edad + " y mi altura " + altura + " cm");

    }
}
```
![[Pasted image 20240305180948.png]]
## Recibir Datos del Usuario - Inputs
En Java, puedes recoger datos del usuario utilizando la clase `Scanner` que se encuentra en el paquete `java.util`. Aquí tienes un ejemplo básico de cómo puedes usarla para recoger datos desde la entrada estándar (teclado):
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        // Crear un objeto Scanner para leer la entrada del usuario
        Scanner scanner = new Scanner(System.in);

        // Solicitar al usuario que ingrese su nombre
        System.out.print("Por favor, ingresa tu nombre: ");
        
        // Leer la entrada del usuario y almacenarla en una variable
        String nombre = scanner.nextLine();

        // Solicitar al usuario que ingrese su edad
        System.out.print("Ingresa tu edad: ");
        
        // Leer la entrada del usuario y almacenarla en una variable
        int edad = scanner.nextInt();

        // Mostrar los datos ingresados por el usuario
        System.out.println("Nombre: " + nombre);
        System.out.println("Edad: " + edad);

        // Cerrar el objeto Scanner para liberar recursos
        scanner.close();
    }
}
```
![[Pasted image 20240304195018.png]]
En este ejemplo, se crea un objeto `Scanner` llamado `scanner` que se utiliza para leer la entrada del usuario. Se solicita al usuario que ingrese su nombre y edad, y luego se almacenan en las variables `nombre` y `edad` respectivamente. Finalmente, se muestran los datos ingresados por el usuario.