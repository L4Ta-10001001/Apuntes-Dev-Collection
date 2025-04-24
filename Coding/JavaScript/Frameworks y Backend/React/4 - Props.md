# EJEMPLO 1
Podemos pasarle una propiedad a un componente, por ejemplo nos ubicamos en nuestro App.tsx:
![[Pasted image 20240707223749.png]]
Lo que hacemos será modificar el código de la siguiente forma:
![[Pasted image 20240707224723.png]]
Y ahora vamos al Card.tsx y vamos a crear una interfaz que se tiene que llamar Props, para poder recibir el dato en sí, que será un String, y luego en la función le pasamos los props y podremos imprimirlo por pantalla:
![[Pasted image 20240707224934.png]]
### PASANDO MÚLTIPLES PROPS
Podemos pasar más de un prop a la vez, por ejemplo dos strings al componente de CardBody:
![[Pasted image 20240707230040.png]]
Y luego en el Card.tsx tendremos que adaptar el componente CardBody de la siguiente forma:
![[Pasted image 20240707230013.png]]
### PROPS OPCIONALES
También podemos usar props opcionales, es decir, por ejemplo podemos elegir no pasar un texto; y para ello usamos una interrogación:
![[Pasted image 20240707230225.png]]
De esta forma, ahora si le pasamos solo el title y no el text, vemos que nos funciona igualmente y no da error:
![[Pasted image 20240707230257.png]]
![[Pasted image 20240707230307.png]]
# EJEMPLO 2
Podemos pasar propiedades desde el main.tsx al PrimerComponente.tsx de la siguiente forma:
![[Pasted image 20240708001308.png]]
Y ahora los recibimos en el PrimerComponente.tsx:
![[Pasted image 20240708001332.png]]
O también podemos pasar una edad, que se enviaría con unas llaves:
![[Pasted image 20240708001544.png]]
Y vemos que funciona:
![[Pasted image 20240708001555.png]]
### PROPTYPES
También podemos usar proptypes para definir el tipo de dato que esperamos recibir y además si va a ser obligatorio o no:
![[Pasted image 20240708094459.png]]
![[Pasted image 20240708094514.png]]
Y podríamos también esperar recibir otro tipo de dato como un tipo number:
![[Pasted image 20240708095021.png]]
Y lo recibimos:
![[Pasted image 20240708095039.png]]
# EJEMPLO 3
Primero, vamos a crear un componente llamado `Greeting` que recibirá una prop llamada `name` y mostrará un saludo personalizado.

**Greeting.jsx**

```javascript
// Greeting.jsx
import React from 'react';

function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

export default Greeting;
```
En este ejemplo, el componente `Greeting` utiliza `props` para acceder a la prop `name` y mostrar un saludo.

Ahora vamos a usar el Componente `Greeting` en el Componente Principal

Ahora, vamos a utilizar el componente `Greeting` en el componente principal de nuestra aplicación, que normalmente se llama `App`.

**App.jsx**

```javascript
// App.jsx
import React from 'react';
import Greeting from './Greeting';

function App() {
  return (
    <div>
      <Greeting name="Alice" />
      <Greeting name="Bob" />
      <Greeting name="Charlie" />
    </div>
  );
}

export default App;
```
En el componente `App`, estamos utilizando el componente `Greeting` tres veces, pasando diferentes valores para la prop `name`.

Finalmente, asegúrate de que tu aplicación esté configurada para renderizar el componente `App` en el main.jsx:

**main.jsx**

```javascript
// main.jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```