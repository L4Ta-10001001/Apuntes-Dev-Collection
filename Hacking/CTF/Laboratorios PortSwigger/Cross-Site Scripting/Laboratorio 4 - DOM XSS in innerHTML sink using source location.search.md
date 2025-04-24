Tenemos el siguiente laboratorio:
![[Pasted image 20240523233150.png]]
Escribimos información y vemos que no pasa nada:
![[Pasted image 20240523233250.png]]
Pero si inyectamos el siguiente payload, vemos que funciona:
```html
<img src=1 onerror=alert(1)>
```
![[Pasted image 20240523233331.png]]
El payload XSS (Cross-Site Scripting) que has proporcionado es un ejemplo básico de un ataque XSS basado en el evento `onerror`. Aquí tienes una explicación detallada de cómo funciona:

1. **Payload**
```bash
<img src=1 onerror=alert(1)>
```
**`<img src=1`**: Este es un intento de insertar una imagen en la página web. El atributo `src` especifica la URL de la imagen. En este caso, se establece en `1`, que no es una URL válida de imagen. Esto está diseñado para provocar un error.

**`onerror=alert(1)`**: El atributo `onerror` es un manejador de eventos en JavaScript que se dispara cuando ocurre un error al cargar la imagen. Aquí se ha añadido código JavaScript para que, cuando ocurra el error (debido a la URL no válida de la imagen), se ejecute `alert(1)`.

**`>`**: Cierra la etiqueta `<img>`.
    
1. **Comportamiento**:
    
    - Cuando el navegador intenta cargar la imagen desde `src=1`, no puede encontrar un recurso válido, lo que provoca un error de carga.
    - Debido a este error, se dispara el manejador de eventos `onerror`.
    - El código dentro de `onerror` se ejecuta, en este caso, `alert(1)`, lo que provoca que el navegador muestre una alerta con el mensaje `1`.