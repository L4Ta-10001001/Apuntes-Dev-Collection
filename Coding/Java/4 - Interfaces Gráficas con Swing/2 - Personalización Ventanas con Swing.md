Con las herencias de la clase JFrame podemos personalizar nuestras interfaces gráficas.

Utilizaremos las clases object, component (con sus métodos setLocation y setBounds), container, windows (con su método seticonimage), frame (con su método setTitle).
![[Pasted image 20240325112039.png]]
## Métodos setLocation, setBounds y setExtendedState
Con el método setLocation podemos configurar la posición de la ventana al abrirse; haciéndolo así:
```java
package com.graficos;
import javax.swing.JFrame;

public class proyecto 
{
    public static void main(String[] args) {
        miMarco marco1 = new miMarco();
        marco1.setVisible(true); 
        marco1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
    }
}

class miMarco extends JFrame{ 

    public miMarco(){

        // setSize(500,300); 
        setLocation(500,300); // En esta posición se abrirá la ventana
    }
}
```
![[Pasted image 20240325112431.png]]
Con el método setBounds podemos configurar el tamaño de la ventana, por ejemplo creando un cuadrado:
```java
package com.graficos;
import javax.swing.JFrame;

public class proyecto
{
    public static void main(String[] args) {
        miMarco marco1 = new miMarco();
        marco1.setVisible(true); 
        marco1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
    }
}

class miMarco extends JFrame{ 

    public miMarco(){

        setSize(500,300); 
        setLocation(500,300);
        setBounds(500,300,250,250); // Regulamos la forma
    }
}
```
![[Pasted image 20240325112803.png]]
También podemos hacer que la ventana no sea redimensionable con setResizable(false)
![[Pasted image 20240325113129.png]]
Para que la pantalla se abra en pantalla completa, usamos setExtendedState:
```java
package com.graficos;
import java.awt.Frame;

import javax.swing.JFrame;

public class proyecto 
{
    public static void main(String[] args) {
        miMarco marco1 = new miMarco();
        marco1.setVisible(true); 
        marco1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
    }
}

class miMarco extends JFrame{ 

    public miMarco(){

        setSize(500,300); 
        setLocation(500,300);
        setBounds(500,300,250,250); 
        setExtendedState(Frame.MAXIMIZED_BOTH); // Pantalla completa
    }
}
```
![[Pasted image 20240325113311.png]]
También podemos regular el título de la ventana:
```java
package com.graficos;
import javax.swing.JFrame;

public class proyecto 
{
    public static void main(String[] args) {
        miMarco marco1 = new miMarco();
        marco1.setVisible(true); 
        marco1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
    }
}

class miMarco extends JFrame{ 

    public miMarco(){

        setSize(500,300); 
        setLocation(500,300);
        setBounds(500,300,250,250); 
        setTitle("Mi Ventana"); // Título de la ventana
    }
}
```
![[Pasted image 20240325113818.png]]
## Centrar la Ventana
Primero necesitamos conocer la resolución de nuestro monitor y así utilizar la clase Toolkit con sus métodos getDefaultToolkit y getScreenSize:
```java
package com.graficos;
import java.awt.Toolkit;
import java.awt.Dimension; 

import javax.swing.JFrame;

public class proyecto 
{
    public static void main(String[] args) {
        MarcoCentrado marco1 = new MarcoCentrado();
        marco1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
        marco1.setVisible(true);
    }
}

class MarcoCentrado extends JFrame{ 

    public MarcoCentrado(){

        Toolkit mipantalla=Toolkit.getDefaultToolkit();
        Dimension tamanoPantalla=mipantalla.getScreenSize(); // Obtenemos dimensión total de la pantalla.
        int alturaPantalla=tamanoPantalla.height; // Obtenemos alto de la pantalla
        int anchoPantalla=tamanoPantalla.width; // Obtenemos ancho de la pantalla
 
        setSize(anchoPantalla/2, alturaPantalla/2); // Nuestra resolución entre 2
        setLocation(anchoPantalla/4, alturaPantalla/4); // Para salir totalmente centrado
    }
}
```
Y ahora obtenemos el marco totalmente centrado:
![[Pasted image 20240325115303.png]]