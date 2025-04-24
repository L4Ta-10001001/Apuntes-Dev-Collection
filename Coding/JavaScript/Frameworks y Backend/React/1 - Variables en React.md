Una vez tengamos nuestro proyecto creado, el primer paso será crear nuestro primer componente que le llamaré PrimerComponente.tsx y después modificar el main.tsx para que quede de esta forma:
```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import PrimerComponente from './PrimerComponente'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <PrimerComponente/>
  </React.StrictMode>,
)
```
![[Pasted image 20240707233321.png]]
Y ahora ya podremos editar nuestro primer componente:
```javascript
const nombre = 'Mario';
const edad = 28;

const PrimerComponente = () => {
  return (
    <p>Tu nombre es {nombre} y tu edad {edad} años</p>
  );
};

export default PrimerComponente;
```
![[Pasted image 20240707233342.png]]
Donde veremos lo siguiente en el navegador:
![[Pasted image 20240707233413.png]]
También podemos imprimir un array:
![[Pasted image 20240707233849.png]]
![[Pasted image 20240707233858.png]]
