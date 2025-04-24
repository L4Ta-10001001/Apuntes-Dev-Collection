Para sacar datos, hemos utilizado la instrucción system.out.printIn, pero también podemos introducir información al programa (un input). Para ello usaremos la clase Scanner con sus clases nextLine(), nextInt() y nextDouble(). Aunque también tenemos la clase JOptionPane con su método showInputDialog(). 

### CLASE SCANNER
Esta clase nos permite recibir información en el terminal utilizando sus distintos métodos, los cuales son los siguientes:

**nextLine(), nextInt() y nextDouble()**
Sirve para especificar a Java que vamos a recoger una cadena de texto:
```java
import java.util.*;

public class codigo {

    public static void main(String[] args) {
                
        // Crear un objeto Scanner para leer la entrada del usuario
        Scanner entrada = new Scanner(System.in);

        // Recogemos el nombre con nextLine ya que se trata de una cadena de texto
        System.out.println("Introduce tu nombre: ");
        String nombreUsuario = entrada.nextLine();

        // Lo mismo de antes pero recogiendo un dato de tipo int
        System.out.println("Introduce tu edad: ");
        int edad = entrada.nextInt();

        
        System.out.println("Hola " + nombreUsuario + ", tienes " + edad + " años");

        // Cerrar el objeto Scanner para liberar los recursos
        entrada.close();
    }
}
```
![[Pasted image 20240307182226.png]]

## CLASE JOPTIONPANE
Es lo mismo que lo anterior pero tiene una interfaz gráfica, aunque dependiendo de si recogemos un string o un número int, su funcionamiento cambia.

**Recogiendo un Dato String**

```java
import javax.swing.JOptionPane;

public class codigo {

    public static void main(String[] args) {
                
        String nombreUsuario =JOptionPane.showInputDialog("Introduce tu nombre: ");
        System.out.println("Hola " + nombreUsuario);

    }
}
```
Se nos abre una ventana donde pide el nombre:
![[Pasted image 20240307183059.png]]
Lo insertamos y se imprime:
![[Pasted image 20240307183111.png]]

**Recogiendo un Dato Int**
Para poder recoger un dato int, primero debemos recogerlo como si se tratase de un string y luego convertirlo a entero de la siguiente forma:
```java
import javax.swing.JOptionPane;

public class codigo {

    public static void main(String[] args) {
                
        String edad =JOptionPane.showInputDialog("Introduce tu edad: "); // Método showInputDialog de la clase JOptionPane
        int edad_corregida=Integer.parseInt(edad); // Recogemos la edad en formato string con el método parseInt de la clase Integer
        System.out.println("Tu edad es " + edad_corregida); // Convertimos la edad a entero

    }
}
```
![[Pasted image 20240307183435.png]]
