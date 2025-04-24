### Datos de Tipo Enteros

En Java, los datos de tipo entero se utilizan para almacenar valores numéricos enteros sin decimales. Existen varios tipos de datos enteros en Java, cada uno con diferentes tamaños y rangos. Aquí te doy una breve explicación de los tipos de datos enteros más comunes: `int`, `short`, `long`, y `byte`.

1. **`int` (entero):**
    
    - Tamaño: 32 bits.
    - Rango: -2^31 (-2,147,483,648) a 2^31-1 (2,147,483,647).
    - Este es el tipo de dato entero más común y se utiliza para la mayoría de las operaciones aritméticas.
    
```java
int numeroEntero = 42;
```


**`short` (entero corto):**

- Tamaño: 16 bits.
- Rango: -2^15 (-32,768) a 2^15-1 (32,767).
- Se utiliza cuando se necesita conservar memoria y el rango del valor es conocido que estará dentro de estos límites más pequeños.

```java
short numeroCorto = 1000;
```

**`long` (entero largo):**

- Tamaño: 64 bits.
- Rango: -2^63 (-9,223,372,036,854,775,808) a 2^63-1 (9,223,372,036,854,775,807).
- Se utiliza cuando se necesitan valores enteros que superan el rango del tipo `int`.

```java
long numeroLargo = 123456789012345L; // La 'L' al final indica que es un literal long.
```

**`byte` (entero de 8 bits):**

- Tamaño: 8 bits.
- Rango: -2^7 (-128) a 2^7-1 (127).
- Es el tipo de dato más pequeño para representar números enteros y se utiliza en situaciones donde la memoria es crítica.

```java
byte numeroByte = 10;
```

-----------------

Vamos a crear la variable myInt con el número 7 y le vamos a sumar un 4, donde al final imprimiremos al valor del resultado:
```java
public class Main {

    public static void main(String[] args) {
                
        int myInt = 7;
        myInt = myInt + 4; # Modificamos la variable.
        System.out.println(myInt);
    
    }
}
```
![[Pasted image 20240304170647.png]]
También podemos cambiar el valor de una variable sobre la marcha sin guardar el dato, como en este caso donde le restamos un 1 a la variable myInt:
```java
public class Main {

    public static void main(String[] args) {
                
        int myInt = 7;
        myInt = myInt + 4;
        System.out.println(myInt);
        System.out.println(myInt -1); 
    
    }
}
```
![[Pasted image 20240304180718.png]]

------------------------
#### DATOS DE TIPO FLOTANTE (DOUBLE y FLOAT)

En Java, `double` y `float` son tipos de datos que representan números de punto flotante con diferentes niveles de precisión.

1. **Rango:**
    
    - `float`: Tiene un rango de valores más limitado en comparación con `double`.
    - `double`: Tiene un rango más amplio y puede representar números mucho más grandes o pequeños que `float`.
2. **Uso de memoria:**
    
    - `float`: Utiliza 4 bytes de memoria.
    - `double`: Utiliza 8 bytes de memoria.
3. **Sufijo de literal:**
    
    - Cuando se asigna un valor literal a una variable de tipo `float`, se debe usar el sufijo 'f' o 'F' para indicar que el valor es de tipo `float`. Por ejemplo: `float ejemplo = 3.14f;`.
    - Los literales sin sufijo se consideran `double` por defecto. Por ejemplo: `double ejemplo = 3.14;`.
4. **Recomendación de uso:**
    
    - Se recomienda usar `double` en la mayoría de los casos, ya que proporciona mayor precisión y es el tipo de punto flotante predeterminado en Java.
    - `float` se utiliza en situaciones donde la limitación de memoria es crítica, como en aplicaciones móviles o sistemas embebidos, y la pérdida de precisión es aceptable.

Para el tipo de dato Double, si queremos imprimir una variable booleana con decimales, utilizamos Double de la siguiente forma:
```java
public class Main {

    public static void main(String[] args) {
                
        Double decimal = 6.5;
        System.out.println(decimal);
    
    }
}
```
![[Pasted image 20240304181012.png]]
Aunque si queremos utilizar un dato de tipo flotante, debemos de especificarlo poniendo una f seguido del número:
```java
public class Main {

    public static void main(String[] args) {
                
        Float flotante = 6.5f; # Con esta f especificamos que se trata de un Float.
        System.out.println(flotante);
    
    }
}
```
![[Pasted image 20240304181418.png]]
De todos modos, podemos operar con ellos y no hay problemas de compatibilidad, por ejemplo probamos en sumarlos:
```java
public class Main {

    public static void main(String[] args) {
                
        Double decimal = 6.5;
        
        Float flotante = 6.5f;
        System.out.println(flotante + decimal);
    
    }
}
```
![[Pasted image 20240304181949.png]]

---------------------

Y así serían todos los tipos de datos al completo:
```java
public class Main {

    public static void main(String[] args) {
                
        long largo = 100000L;
        byte byteVariable = 127;
        short corto = 32000;

        System.out.println("Valor de largo: " + largo);
        System.out.println("Valor de byteVariable: " + byteVariable);
        System.out.println("Valor de corto: " + corto);
    }
}
```
![[Pasted image 20240304193633.png]]
#### DATOS DE TIPO BOOLEAN

En Java, el tipo de dato `boolean` se utiliza para representar valores de verdad, es decir, valores lógicos que pueden ser `true` (verdadero) o `false` (falso). Los booleanos son fundamentales para controlar la lógica de las decisiones en los programas. Aquí tienes una explicación del tipo de dato booleano:

- **`boolean`:**
    
    - Tamaño: No se especifica exactamente en términos de bits, pero suele ser representado de manera eficiente en memoria.
    - Valores posibles: `true` o `false`.
    - Se utiliza para expresar condiciones lógicas y tomar decisiones en base a ellas.

```java
boolean esVerdadero = true;
boolean esFalso = false;
```

**Ejemplo en una estructura de control:**

```java
int edad = 18;
boolean esAdulto = (edad >= 18);

if (esAdulto) {
    System.out.println("Es un adulto");
} else {
    System.out.println("No es un adulto");
}
```
En este ejemplo, la variable `esAdulto` se evalúa como `true` o `false` dependiendo de si la `edad` es mayor o igual a 18. La estructura `if` utiliza esta variable booleana para decidir qué mensaje imprimir en la consola.
