La clase `Math` en Java es una clase que proporciona métodos estáticos para realizar operaciones matemáticas comunes. Estos métodos son útiles cuando necesitas realizar cálculos matemáticos en tu programa. En el caso de la clase `Math` en Java, no se trata de una clase que puedas instanciar para crear objetos, sino que es una clase que proporciona métodos estáticos, lo que significa que puedes llamar a estos métodos directamente en la clase sin necesidad de crear una instancia de la misma.
**Método sqrt**
```java
public class codigo {

    public static void main(String[] args) {
                
        double raiz=Math.sqrt(9); // Método sqrt para calcular la raíz cuadrada.
        System.out.println(raiz);
        
    }
}
```
![[Pasted image 20240305183643.png]]
**Método floor**
Para redondear un número hacia abajo:
![[Pasted image 20240305183831.png]]
**Método ceil**
Lo mismo pero para redondear hacia arriba:
![[Pasted image 20240305183925.png]]
**Método round**
Lo mismo pero redondea al número más próximo:
![[Pasted image 20240305184039.png]]
**Método abs**
Para convertir un número negativo a absoluto:
![[Pasted image 20240305184150.png]]
**Método Pow**
Sirve para calcular la potencia de un número, donde además también podemos hacer cálculos directamente y guardarlos en la variable resutado:
![[Pasted image 20240307110914.png]]
