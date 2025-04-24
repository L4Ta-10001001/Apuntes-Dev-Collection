# CREAR UN COMPONENTE
Vamos a crear un componente saludo:
![[Pasted image 20240707222214.png]]
Ahora que ya tenemos definido nuestro componente, podremos utilizarlo en cualquier parte de nuestra aplicación React:
![[Pasted image 20240707222259.png]]
Y esto es lo que veremos en el navegador:
![[Pasted image 20240707222319.png]]



### COMPONENTE PADRE
Son las unidades fundamentales con las que se construye una interfaz de usuario, y lo crearemos en el App.tsx que se llamará App(), donde tendremos el siguiente código:
```javascript
function App() {

  const nombre = 'Mario'
  return <p>Hola {nombre}</p>; // Esto es código jsx

}

export default App;
```
![[Pasted image 20240707202823.png]]
### COMPONENTE HIJO
También podemos crear otro componente y luego importarlo en la App(), por ejemplo, vamos a crear un archivo llamado titulo.tsx donde tenga el código anterior:
![[Pasted image 20240707203754.png]]
Y luego podemos importar este componente en nuestro App():
![[Pasted image 20240707203823.png]]
# COMPONENTE CARD
Vamos a crear un directorio llamado components y en su interior un Card.tsx
![[Pasted image 20240707205550.png]]
E iremos a la web de bootstrap y buscaremos el apartado Card, donde copiaremos ese código y lo pegaremos en el Card.tsx que hemos creado antes:
```bash
https://getbootstrap.com/docs/5.3/components/card/
```
![[Pasted image 20240707205312.png]]
Una vez copiado, lo pegaremos dentro de una función que se llame como el archivo, aunque lo cambiaremos un poco dejándolo de la siguiente forma:
![[Pasted image 20240707205621.png]]
Y este componente podremos ahora añadirlo dentro del directorio /components al componente padre de App():
![[Pasted image 20240707205829.png]]
### ASIGNAR ESTILOS A LOS COMPONENTES
De esta forma, si yo quisiera por ejemplo aplicar un estilo a esta Card, podríamos hacerlo directamente dentro del componente Card.tsc, añadiendo un style dentro del div:
![[Pasted image 20240707210153.png]]
Aunque también podemos dejarlo más bonito de la siguiente forma:
![[Pasted image 20240707210428.png]]
### CREAR UN COMPONENTE DENTRO DE OTRO
Por ejemplo, en nuestro caso tenemos todo en un mismo componente, pero podríamos querer tener el componente de Card y luego otro que sea CardBody para guardar únicamente el texto. De tal forma que los estilos se aplican desde el componente Card, y el texto se aplica desde el sub-componente CardBody:
```javascript
function Card() {
  return (
    <div className="card" style={{ width: "350px" }}>
      <div className="card-body">
       <CardBody />
      </div>
    </div>
  );
}

export default Card;

export function CardBody() {
  return (
    <div>
      <h5 className="card-title">Card title</h5>
      <p className="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
      <a href="#" className="btn btn-primary">Go somewhere</a>
    </div>
  );
}
```
![[Pasted image 20240707211816.png]]
Y vemos que el componente sigue funcionando igual, pero con la diferencia que lo hemos dividido:
![[Pasted image 20240707211849.png]]
