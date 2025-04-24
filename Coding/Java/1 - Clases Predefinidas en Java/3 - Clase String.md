Esta clase se utiliza para representar cadenas de caracteres, que son secuencias de caracteres. String no es un tipo de datos como sí ocurría con tipos primitivos como int op double, sino que string es una clase, aunque en la práctica se utiliza de una forma similar para almacenar cadenas de caracteres. Aquí tenemos un ejemplo de cómo usar la clase String:
```java
public class codigo {

    public static void main(String[] args) {
                
        String Nombre="Mi nombre es Mario";
        System.err.println(Nombre);
        
    }
}
```
![[Pasted image 20240307112419.png]]
## Métodos más comunes de la clase String

Al tratarse de una clase, esta ya cuenta con métodos para poder manipular cadenas de caracteres. Por ejemplo podemos saber longitud de una cadena de caracteres, posición, etc:

**Método length()**
Sirve para conocer cuantos caracteres tiene una cadena de texto. Donde podemos pasarle el método length() al objeto nombre (que al estar con la clase String, se trata de un objeto y no de una variable) y así obtener su longitud:
```java
public class codigo {

    public static void main(String[] args) {
                
        String Nombre="Mario";
        System.out.println("Mi nombre tiene " + Nombre.length() + " letras.");
        
    }
}
```
![[Pasted image 20240307112642.png]]


**Método charAt()**
Sirve para conocer la posición donde se encuentra un determinado patrón:
```java
public class codigo {

    public static void main(String[] args) {
                
        String Nombre="Mario";
        System.out.println("La primera letra de mi nombre es: " + Nombre.charAt(0));
        
    }
}
```
![[Pasted image 20240307112930.png]]
O incluso podemos guardar en una variable el resultado de aplicar el método charAt al objeto nombre:
```java
public class codigo {

    public static void main(String[] args) {
                
        String nombre = "Mario";
        char primera_letra;
        primera_letra = nombre.charAt(0);
        
        System.out.println("La primera letra de mi nombre es: " + primera_letra);
        
    }
}
```
![[Pasted image 20240307113321.png]]
**Método substring(x,y)**
Sirve para obtener un rango de caracteres de una cadena de texto. Por ejemplo desde el caracter 0 hasta el número 10:
```java
public class codigo {

    public static void main(String[] args) {
                
        String frase="Esta es una frase donde vamos a filtrar información";
        String filtro_frase=frase.substring(0, 10);

        System.out.println(filtro_frase);
        
    }
}
```
![[Pasted image 20240307113931.png]]
Incluso también podemos imprimir un determinado número de caracteres y luego concatenarlo con el texto que queramos. Por ejemplo, vamos a imprimir los 11 primeros caracteres y luego una frase de "Cadena de texto diferente":
```java
public class codigo {

    public static void main(String[] args) {
                
        String frase="Esta es una frase donde vamos a filtrar información";
        String filtro_frase=frase.substring(0, 11) + " Cadena de texto diferente";

        System.out.println(filtro_frase);
        
    }
}
```
![[Pasted image 20240307114215.png]]
**Método equals()**
Sirve para comparar cadenas de caracteres, donde nos devolverá true si es verdadero, y false si es falso:
```java
public class codigo {

    public static void main(String[] args) {
                
        String alumno1, alumno2;

        alumno1="Mario";
        alumno2="Mario";

        System.out.println(alumno1.equals(alumno2));
        
    }
}
```
![[Pasted image 20240307114624.png]]
**Método equalsIgnoreCase()**
Sirve para lo mismo de antes pero ignorando las mayúsculas y minúsculas:
```java
public class codigo {

    public static void main(String[] args) {
                
        String alumno1, alumno2;

        alumno1="Mario";
        alumno2="mario";

        System.out.println(alumno1.equalsIgnoreCase(alumno2));
        
    }
}
```
![[Pasted image 20240307114720.png]]
