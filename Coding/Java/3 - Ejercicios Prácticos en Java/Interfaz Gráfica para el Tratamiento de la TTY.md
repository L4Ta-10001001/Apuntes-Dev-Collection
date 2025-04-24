

```java
import javax.swing.*;
import java.awt.*;
import java.awt.datatransfer.StringSelection;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Main {
    public static void main(String[] args) {
        // Crear el JFrame principal
        JFrame frame = new JFrame("Aplicación con Swing");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 400);

        // Crear el panel principal con BoxLayout en Y_AXIS
        JPanel mainPanel = new JPanel();
        mainPanel.setLayout(new BoxLayout(mainPanel, BoxLayout.Y_AXIS));

        // Crear los paneles con FlowLayout
        JPanel panel1 = new JPanel(new FlowLayout());
        JPanel panel2 = new JPanel(new FlowLayout());
        JPanel panel3 = new JPanel(new FlowLayout());
        JPanel panel4 = new JPanel(new FlowLayout());
        JPanel panel5 = new JPanel(new FlowLayout());

        // Crear los botones con sus textos correspondientes
        JButton button1 = new JButton("Paso 1");
        JButton button2 = new JButton("Paso 2");
        JButton button3 = new JButton("Paso 3");
        JButton button4 = new JButton("Paso 4");
        JButton button5 = new JButton("Paso 5");

        // Añadir los botones a los paneles
        panel1.add(button1);
        panel2.add(button2);
        panel3.add(button3);
        panel4.add(button4);
        panel5.add(button5);

        // Añadir los paneles al panel principal
        mainPanel.add(panel1);
        mainPanel.add(panel2);
        mainPanel.add(panel3);
        mainPanel.add(panel4);
        mainPanel.add(panel5);

        // Añadir el panel principal al frame
        frame.add(mainPanel);

        // Crear ActionListeners para los botones
        button1.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                copiarTextoYMostrarMensaje("script /dev/null -c bash", frame);
            }
        });

        button2.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                copiarTextoYMostrarMensaje("Pulsa control C", frame);
            }
        });

        button3.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                copiarTextoYMostrarMensaje("stty raw -echo; fg", frame);
            }
        });

        button4.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                copiarTextoYMostrarMensaje("reset xterm", frame);
            }
        });

        button5.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                copiarTextoYMostrarMensaje("export SHELL=bash && export TERM=xterm", frame);
            }
        });

        // Hacer visible el frame
        frame.setVisible(true);
    }

    // Método para copiar texto al portapapeles y mostrar un mensaje
    public static void copiarTextoYMostrarMensaje(String mensaje, JFrame frame) {
        // Copiar el mensaje al portapapeles
        StringSelection stringSelection = new StringSelection(mensaje);
        Toolkit.getDefaultToolkit().getSystemClipboard().setContents(stringSelection, null);
        // Mostrar mensaje al usuario
        JOptionPane.showMessageDialog(frame, mensaje);
    }
}
```
Obtendremos la siguiente aplicación:
![[Pasted image 20240520124659.png]]
Donde si hacemos clic en cada uno de los pasos, se nos mostrará el comando que tenemos que pegar en cada momento:
![[Pasted image 20240520124726.png]]
