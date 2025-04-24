## EJEMPLO 1

El primer paso será crear un archivo llamado Greeting.jsx:
```javascript
import React from 'react'


const Greeting = ({ isLoggedIn }) => {
  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  } else {
    return <h1>Please sign in.</h1>;
  }
};

export default Greeting;
```
![[Pasted image 20240708184419.png]]
A continuación importamos el componente que hemos creado antes de Greeting y añadimos el código en App.jsx que evalúe si el usuario está logueado o no tras presionar el botón:
```javascript
import React, { useState } from 'react';
import Greeting from './components/Greeting';

const App = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const handleLoginClick = () => {
    setIsLoggedIn(true);
  };

  const handleLogoutClick = () => {
    setIsLoggedIn(false);
  };

  let button;
  if (isLoggedIn) {
    button = <button onClick={handleLogoutClick}>Logout</button>;
  } else {
    button = <button onClick={handleLoginClick}>Login</button>;
  }

  return (
    <div>
      <Greeting isLoggedIn={isLoggedIn} />
      {button}
    </div>
  );
};

export default App;
```
![[Pasted image 20240708184538.png]]
Por último, dentro del main.jsx cargamos la aplicación:
![[Pasted image 20240708184559.png]]
## EJEMPLO 2
Supongamos que queremos mostrar diferentes mensajes y botones basados en si el usuario es un administrador o un usuario regular. Por lo que vamos a crear un componente `UserGreeting` que mostrará un mensaje diferente basado en si el usuario es un administrador o un usuario regular:
```javascript
// src/components/UserGreeting.js

import React from 'react';

const UserGreeting = ({ isAdmin }) => {
  if (isAdmin) {
    return <h1>Welcome, Admin!</h1>;
  } else {
    return <h1>Welcome, User!</h1>;
  }
};

export default UserGreeting;
```
![[Pasted image 20240708184831.png]]
Vamos a crear un componente `App` que controle el estado de si el usuario es un administrador o no, y tenga botones para cambiar este estado.
```javascript
import React, { useState } from 'react';
import UserGreeting from './components/UserGreeting';

const App = () => {
  const [isAdmin, setIsAdmin] = useState(false);

  const handleAdminClick = () => {
    setIsAdmin(true);
  };

  const handleUserClick = () => {
    setIsAdmin(false);
  };

  let button;
  if (isAdmin) {
    button = <button onClick={handleUserClick}>Switch to User</button>;
  } else {
    button = <button onClick={handleAdminClick}>Switch to Admin</button>;
  }

  return (
    <div>
      <UserGreeting isAdmin={isAdmin} />
      {button}
    </div>
  );
};

export default App;
```
![[Pasted image 20240708184902.png]]
Por último, dentro del main.jsx cargamos la aplicación:
![[Pasted image 20240708184559.png]]
1. **Componente `UserGreeting`:** Este componente recibe una propiedad (`isAdmin`). Basado en su valor (`true` o `false`), muestra un mensaje diferente.
    
2. **Componente `App`:**
    
    - Mantiene el estado de `isAdmin` usando el hook `useState`.
    - Tiene dos funciones (`handleAdminClick` y `handleUserClick`) para alternar el estado de `isAdmin`.
    - Usa una sentencia condicional `if` para determinar qué botón mostrar (Switch to User o Switch to Admin).
    - Renderiza el componente `UserGreeting` pasando el estado `isAdmin` como una prop.
    - Muestra el botón correspondiente basado en el estado actual de `isAdmin`.

Con esta estructura, puedes ver cómo se renderiza el mensaje apropiado y el botón basado en si el usuario es un administrador o no. Esta práctica te ayuda a repasar el uso de condicionales en React y cómo manejarlos en la interfaz de usuario.