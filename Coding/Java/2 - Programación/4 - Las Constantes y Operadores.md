**CONSTANTES**
Las constantes son espacios en la memoria del ordenador donde se almacenará un valor que no podrá cambiar durante la ejecución del programa. Las constantes se crean con la palabra reservada final:
```java
public class codigo {
    public static void main(String args[]) {
        
        final String constante="Texto dentro de una constante";
        System.out.println(constante);

    }
}
```
Y se imprime la constante:
![[Pasted image 20240305180038.png]]
Diferencias entre una constante y una variable:


**OPERADORES**
En Java, los operadores lógicos se utilizan para realizar operaciones booleanas entre expresiones o valores booleanos. Los operadores lógicos más comunes en Java son:
**AND lógico (`&&`):**

- Sintaxis: `expresion1 && expresion2`
- Devuelve `true` si ambas expresiones son verdaderas; de lo contrario, devuelve `false`.

```java
boolean resultado = (5 > 3) && (10 < 20); // true
```
**OR lógico (`||`):**

- Sintaxis: `expresion1 || expresion2`
- Devuelve `true` si al menos una de las expresiones es verdadera; devuelve `false` si ambas son falsas.

```java
boolean resultado = (5 > 3) || (10 > 20); // true
```
**NOT lógico (`!`):**

- Sintaxis: `!expresion`
- Devuelve `true` si la expresión es falsa, y viceversa.

```java
boolean resultado = !(5 > 3); // false
```
Estos operadores son fundamentales para construir expresiones lógicas más complejas y tomar decisiones en base a condiciones en programas Java.

**Ejemplo de uso en una declaración `if`:**

```java
public class codigo {
    public static void main(String args[]) {
        
        int edad = 25;

        if (edad >= 18 && edad <= 30) {
            System.out.println("La persona es mayor de edad y tiene menos de 30 años.");
        } else {
            System.out.println("La persona no cumple con las condiciones especificadas.");
}   

    }
}
```
