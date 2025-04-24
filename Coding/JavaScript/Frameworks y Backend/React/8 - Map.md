En React, el método `map` de JavaScript se utiliza comúnmente para renderizar listas de elementos. `map` es un método de los arrays que permite iterar sobre cada elemento de un array y aplicar una función de transformación a cada uno, devolviendo un nuevo array con los resultados.

```javascript
import React, { useState } from 'react';

const Items = ({ nombre, visto }) => {
    return (
        <li>{nombre} {visto && '✅'}</li>
    );
};

export const ListadoApp = () => {
    const listadoSecciones = [
        { nombre: 'Instalaciones necesarias', visto: true },
        { nombre: 'Uso de Vite', visto: true },
        { nombre: 'Componentes', visto: true },
        { nombre: 'Variables en JSX', visto: true },
        { nombre: 'Props', visto: true },
        { nombre: 'Eventos', visto: true },
        { nombre: 'useState', visto: true },
        { nombre: 'Redux', visto: false },
        { nombre: 'customHooks', visto: false },
    ];

    const [arreglo, setArreglo] = useState(listadoSecciones);
    
    return (
        <div>
            <h1>Listado de Temas del Curso</h1>
            <ol>
                {arreglo.map(item => (
                    <Items key={item.nombre} nombre={item.nombre} visto={item.visto} />
                ))}
            </ol>
        </div>
    );
};
```
### OTRO EJEMPLO


```javascript
import React from 'react';

// Componente Tarea que recibe las props 'nombre' y 'completado'
const Tarea = ({ nombre, completado }) => {
    return (
        <li>
            {nombre} {completado ? '✅' : '❌'}
        </li>
    );
};

// Componente ListaDeTareas que contiene el array de tareas y usa 'map' para renderizarlas
const ListaDeTareas = () => {
    // Array de tareas
    const tareas = [
        { id: 1, nombre: 'Lavar los platos', completado: true },
        { id: 2, nombre: 'Sacar la basura', completado: false },
        { id: 3, nombre: 'Hacer la cama', completado: true }
    ];

    return (
        <div>
            <h1>Lista de Tareas</h1>
            <ul>
                {tareas.map(tarea => (
                    // Renderizar un componente Tarea para cada objeto en el array tareas
                    <Tarea key={tarea.id} nombre={tarea.nombre} completado={tarea.completado} />
                ))}
            </ul>
        </div>
    );
};

// Exportar el componente ListaDeTareas como el componente predeterminado
export default ListaDeTareas;
```
Así tendríamos el main.jsx:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './styles.css';
import ListaDeTareas from './Tarea.jsx';

ReactDOM.createRoot(document.getElementById('root')).render(
    <React.StrictMode>
        <ListaDeTareas />
    </React.StrictMode>,
);
```
Y así se vería donde se recorre cada elemento del array con map:
![[Pasted image 20240714115736.png]]
### OTRO EJEMPLO
Así tendríamos el Tarea.jsx:
```javascript
import React from 'react';

const NumerosLista = () => {
    // Array de números
    const numeros = [1, 2, 3, 4, 5];

    return (
        <div>
            <h2>Números</h2>
            <ul>
                {numeros.map(numero => (
                    <li key={numero}>
                        <p>En esta vuelta estamos recorriendo el número {numero}</p>
                    </li>
                ))}
            </ul>
        </div>
    );
};

export default NumerosLista;
```
Y así se vería:
![[Pasted image 20240714120413.png]]

