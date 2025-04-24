 Vamos a crear una aplicación gráfica similar a esta:
  ![[Pasted image 20240527195957.png]]
Haríamos el siguiente código:
  ```java
import javax.swing.*;  
import java.awt.event.ActionEvent;  
import java.awt.event.ActionListener;  
  
public class Ventana {  
    public static void main(String[] args) {  
        JFrame frame = new JFrame();  
        frame.setSize(400, 200);  
  
        JPanel panel = new JPanel();
        frame.add(panel);  
        panel.setLayout(null);  
  
        JLabel label1 = new JLabel("Texto 1");  
        label1.setBounds(10, 20, 80, 25);  
        panel.add(label1);  
  
        JLabel label2 = new JLabel("Texto 2");  
        label2.setBounds(10, 50, 80, 25);  
        panel.add(label2);  
  
        JLabel label3 = new JLabel("Texto 3");  
        label3.setBounds(10, 80, 80, 25);  
        panel.add(label3);  
  
        JLabel LabelResultado = new JLabel("Resultado");  
        LabelResultado.setBounds(230, 110, 150, 25);  
        panel.add(LabelResultado);  
  
        JTextField text1 = new JTextField(20);  
        text1.setBounds(100, 20, 165, 25);  
        panel.add(text1);  
  
        JTextField text2 = new JTextField(20);  
        text2.setBounds(100, 50, 165, 25);  
        panel.add(text2);  
  
        JTextField text3 = new JTextField(20);  
        text3.setBounds(100, 80, 165, 25);  
        panel.add(text3);  
  
        JButton botonsuma = new JButton("Sumar");  
        botonsuma.setBounds(10, 110, 100, 25);  
        panel.add(botonsuma);  
  
        JButton botonresta = new JButton("Restar");  
        botonresta.setBounds(120, 110, 100, 25);  
        panel.add(botonresta);  
  
        botonsuma.addActionListener(new ActionListener(){  
            @Override  
            public void actionPerformed(ActionEvent e) {  
                try {  
                    int num1 = Integer.parseInt(text1.getText());  
                    int num2 = Integer.parseInt(text2.getText());  
                    int num3 = Integer.parseInt(text3.getText());  
                    int resultado = num1 + num2 + num3;  
                    LabelResultado.setText("Resultado: " + resultado);  
                } catch (NumberFormatException error) {  
                    LabelResultado.setText("Ingrese datos válidos");  
                }  
  
            }  
        });  
  
        botonresta.addActionListener(new ActionListener(){  
            @Override  
            public void actionPerformed(ActionEvent e) {  
                try {  
                    int num1 = Integer.parseInt(text1.getText());  
                    int num2 = Integer.parseInt(text2.getText());  
                    int num3 = Integer.parseInt(text3.getText());  
                    int resultado = num1 - num2 - num3;  
                    LabelResultado.setText("Resultado: " + resultado);  
                } catch (NumberFormatException error) {  
                    LabelResultado.setText("Ingrese datos válidos");  
                }  
            }  
        });  
  
        frame.setVisible(true);  
    }  
}
```
![[Pasted image 20240527205042.png]]
Y este sería el diagrama UML:
![[Pasted image 20240627123322.png]]
