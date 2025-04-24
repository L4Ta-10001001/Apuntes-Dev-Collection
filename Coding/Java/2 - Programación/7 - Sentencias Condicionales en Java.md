Las sentencias condicionales en Java funcionan de la siguiente forma. Las cuales deben de contar con llaves de apertura y cierre en cada una de ellas; y cuentan con la siguiente sintaxis:
```java
import javax.swing.JOptionPane;

public class codigo {

    public static void main(String[] args) {
                
        String edad_string =JOptionPane.showInputDialog("Introduce tu edad: ");
        int edad_corregida=Integer.parseInt(edad_string); 
        
        if (edad_corregida<18){
            System.out.println("Eres menor de edad"); // Si el usuario inserta un número inferior a 18
        }

        else if(edad_corregida<40){
            System.out.println("Eres joven"); // Si el usuario inserta un número inferior a 40
        }

        else{
            System.out.println("Eres adulto"); // Si el usuario inserta cualquier otro número que no entre en lo anterior
        }

    }
}
```
![[Pasted image 20240307184839.png]]
![[Pasted image 20240307184824.png]]
# CONDICIONALES CON CASE
En Java, las sentencias condicionales se implementan principalmente con `if`, `else if` y `else`. Sin embargo, también hay una forma de implementar condiciones múltiples utilizando la declaración `switch`. Aquí te dejo un ejemplo de cómo usar `switch` en Java:
```java
public class codigo {

    public static void main(String[] args) {
        int opcion = 2; // Guardamos un 2 en la variable opción

        switch (opcion) {
            case 1:
                System.out.println("Seleccionaste la opción 1");
                break;
            case 2:
                System.out.println("Seleccionaste la opción 2"); // Entraremos en este case porque la variable opción vale 2
                break;
            case 3:
                System.out.println("Seleccionaste la opción 3");
                break;
            default: // Con el Default se ejecuta en caso de no entrar en ninguno de los case anteriores
                System.out.println("Opción no válida");
        }
    }
}
```
![[Pasted image 20240307202711.png]]
Es importante notar que después de ejecutar un bloque de código, se utiliza `break` para salir del `switch`.
