Las **funciones anónimas** en JavaScript son funciones que no tienen un nombre definido. Son útiles en situaciones donde se necesita una función temporal o se desea pasar una función como argumento a otra función. Estas funciones se pueden asignar a variables, usar como callbacks, o definir directamente en la invocación de otra función. A continuación, te presento algunos ejemplos de cómo funcionan las funciones anónimas en JavaScript.
**Definición y Asignación a Variables**

```javascript
const suma = function(a, b) {
    return a + b;
};

console.log(suma(5, 3)); // Salida: 8
```


**Funciones Callback**
una **función callback** es una función que se pasa como argumento a otra función y se ejecuta después de que esa función haya terminado su trabajo

```javascript
function procesarDato(dato, callback) {
    console.log("Procesando dato: " + dato);
    callback();
}

function finalizarProceso() {
    console.log("Proceso finalizado.");
}

// Llamamos a la función `procesarDato` con el callback `finalizarProceso`
procesarDato("12345", finalizarProceso);
```
