### 1. **Swing**

**Swing** es una biblioteca de Java que proporciona un conjunto de componentes GUI (Graphical User Interface) para crear aplicaciones de escritorio. Swing es parte de Java Foundation Classes (JFC) y ofrece una serie de elementos como botones, cuadros de texto, tablas, y más, que permiten la creación de interfaces de usuario ricas y personalizables. Swing es independiente de la plataforma, lo que significa que las aplicaciones creadas con Swing tendrán el mismo aspecto y comportamiento en cualquier sistema operativo.

#### Características de Swing:

- **Completamente escrito en Java**: Esto hace que sea independiente de la plataforma.
- **Ligero**: Los componentes de Swing no dependen de los componentes nativos del sistema operativo.
- **Extensible**: Permite la personalización y extensión de componentes existentes.
- **MVC**: Utiliza el patrón Modelo-Vista-Controlador para separar la lógica de negocio de la interfaz de usuario.

### 2. **JPanel**

**JPanel** es un contenedor ligero que puede ser utilizado para organizar otros componentes dentro de un GUI. Los `JPanel` son generalmente utilizados para agrupar componentes y aplicarles un `LayoutManager` específico para controlar su disposición.

#### Características de JPanel:

- **Contenedor genérico**: Útil para contener y organizar otros componentes.
- **LayoutManager**: Puede tener un `LayoutManager` asignado para gestionar el diseño de los componentes hijos.
- **Ligero**: No es una ventana en sí misma, sino un contenedor para otros componentes dentro de un `JFrame` u otro contenedor.

### 3. **JFrame**

**JFrame** es una ventana con un borde y un título que puede contener otros componentes GUI. `JFrame` es la base para la mayoría de las aplicaciones de Swing que requieren una ventana principal. A diferencia de `JPanel`, `JFrame` puede existir como una ventana independiente en el escritorio.

#### Características de JFrame:

- **Ventana principal**: Es comúnmente la ventana principal de una aplicación Swing.
- **Decoraciones**: Puede tener bordes, un título, botones de minimizar, maximizar y cerrar.
- **Root Pane**: Contiene un `JRootPane` que a su vez contiene un `ContentPane` para agregar componentes.

### 4. **Layouts**

**Layouts** en Swing son gestionadores de disposición que controlan cómo los componentes se organizan dentro de un contenedor. Los `LayoutManager` son interfaces que se implementan para definir la estrategia de disposición de los componentes.

#### Tipos Comunes de Layouts:

- **FlowLayout**: Coloca los componentes en una fila, comenzando una nueva fila si es necesario.
- **BorderLayout**: Divide el contenedor en cinco regiones: norte, sur, este, oeste y centro.
- **BoxLayout**: Organiza los componentes en una sola fila o columna.
- **GridLayout**: Organiza los componentes en una cuadrícula con celdas de igual tamaño.
- **GridBagLayout**: Un gestor de disposición más complejo y flexible que permite especificar restricciones para cada componente.

### Diferencias:

- **Swing vs JPanel/JFrame/Layout**:
    
    - **Swing** es la biblioteca completa que incluye todas las clases necesarias para crear GUIs.
    - **JPanel** y **JFrame** son clases específicas dentro de la biblioteca Swing.
    - **Layouts** son mecanismos que Swing proporciona para organizar componentes dentro de contenedores como `JPanel` y `JFrame`.
- **JPanel vs JFrame**:
    
    - **JPanel** es un contenedor ligero utilizado para agrupar otros componentes. No puede existir por sí solo como una ventana independiente.
    - **JFrame** es una ventana principal independiente que puede contener componentes, incluyendo `JPanel`.
- **JPanel/JFrame vs Layouts**:
    
    - **JPanel** y **JFrame** son contenedores que pueden tener un `LayoutManager` asignado para organizar sus componentes.
    - **Layouts** son responsables de definir cómo se colocan los componentes dentro de un contenedor.

---------------

Los elementos de interfaz de pintan sobre una ventana. 
```java
package com.graficos;
import javax.swing.JFrame;

public class proyecto 
{
    public static void main(String[] args) {
        miMarco marco1 = new miMarco(); // Creamos una instancia de la clase miMarco
        marco1.setVisible(true); // Hacemos visible el marco en la interfaz
        marco1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // Configuramos el comportamiento al cerrar la ventana
    }
}

class miMarco extends JFrame{ // Extendemos la clase JFrame de Swing para crear nuestra ventana

    public miMarco(){ // Constructor

        setSize(500,300); // Establecemos el tamaño de la ventana
    }
}
```
![[Pasted image 20240325111804.png]]

### Layouts en Java Swing

En Java Swing, los layouts son fundamentales para organizar los componentes en un contenedor. Aquí te proporciono ejemplos de cómo usar `FlowLayout`, `BorderLayout` y `BoxLayout` en aplicaciones Java.
#### 1. Ejemplo con `FlowLayout`
`FlowLayout` coloca los componentes uno al lado del otro en una fila y, cuando la fila se llena, se pasa a la siguiente fila.
```java
import javax.swing.*;
import java.awt.*;

public class Main extends JFrame {

    public Main() {
        setTitle("FlowLayout Example");
        setSize(400, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // Centra la ventana en la pantalla

        // Establecer FlowLayout
        setLayout(new FlowLayout());

        // Añadir botones al JFrame
        add(new JButton("Button 1"));
        add(new JButton("Button 2"));
        add(new JButton("Button 3"));
        add(new JButton("Button 4"));
        add(new JButton("Button 5"));
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            Main frame = new Main();
            frame.setVisible(true);
        });
    }
}
```
Este sería el resultado:
![[Pasted image 20240520094623.png]]
#### 2. Ejemplo con `BorderLayout`
`BorderLayout` divide el contenedor en cinco áreas: norte, sur, este, oeste y centro. Cada área puede contener solo un componente, y se redimensiona para ajustarse a la región asignada.
```java
import javax.swing.*;
import java.awt.*;

public class Main extends JFrame {

    public Main() {
        setTitle("BorderLayout Example");
        setSize(400, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // Centra la ventana en la pantalla

        // Establecer BorderLayout
        setLayout(new BorderLayout());

        // Añadir botones al JFrame en diferentes regiones
        add(new JButton("North"), BorderLayout.NORTH);
        add(new JButton("South"), BorderLayout.SOUTH);
        add(new JButton("East"), BorderLayout.EAST);
        add(new JButton("West"), BorderLayout.WEST);
        add(new JButton("Center"), BorderLayout.CENTER);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            Main frame = new Main();
            frame.setVisible(true);
        });
    }
}
```
![[Pasted image 20240520094720.png]]
#### 3. Ejemplo con `BoxLayout`
`BoxLayout` organiza los componentes en una sola fila o columna.
```java
import javax.swing.*;
import java.awt.*;

public class Main extends JFrame {

    public Main() {
        setTitle("BoxLayout Example");
        setSize(400, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // Centra la ventana en la pantalla

        // Crear un panel con BoxLayout en el eje Y (vertical)
        JPanel panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));

        // Añadir botones al panel
        panel.add(new JButton("Button 1"));
        panel.add(new JButton("Button 2"));
        panel.add(new JButton("Button 3"));
        panel.add(new JButton("Button 4"));
        panel.add(new JButton("Button 5"));

        // Añadir el panel al JFrame
        add(panel);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            Main frame = new Main();
            frame.setVisible(true);
        });
    }
}
```
![[Pasted image 20240520094806.png]]
## ActionListener
Si queremos que se haga una determinada acción al pulsar un botón, debemos de usar ActionListener:
```java
import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Main extends JFrame {

    public Main() {
        setTitle("BoxLayout Example");
        setSize(400, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // Centra la ventana en la pantalla

        // Crear un panel con BoxLayout en el eje Y (vertical)
        JPanel panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));

        // Crear los botones
        JButton button1 = new JButton("Button 1");
        JButton button2 = new JButton("Button 2");
        JButton button3 = new JButton("Button 3");
        JButton button4 = new JButton("Button 4");
        JButton button5 = new JButton("Button 5");

        panel.add(button1);
        panel.add(button2);
        panel.add(button3);
        panel.add(button4);
        panel.add(button5);

        add(panel);

        // Añadir ActionListener a Button 1
        button1.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Texto del botón 1");
            }
        });
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            Main frame = new Main();
            frame.setVisible(true);
        });
    }
}
```
![[Pasted image 20240520095926.png]]

