Tenemos la siguiente base de datos en MySQL:
![[Pasted image 20240705110347.png]]
Y tenemos el siguiente código para hacer la conexión:
```javascript
// Importar el módulo mysql
const mysql = require('mysql');

// Configuración de la conexión a la base de datos
const connection = mysql.createConnection({
    host: 'localhost',  // Cambiar por tu host
    user: 'root', // Cambiar por tu usuario de MySQL
    password: '123123', // Cambiar por tu contraseña de MySQL
    database: 'prueba' // Cambiar por el nombre de tu base de datos
});

// Conectar a la base de datos
connection.connect((err) => {
    if (err) {
        console.error('Error de conexión: ' + err.stack);
        return;
    }
    console.log('Conexión exitosa con el ID ' + connection.threadId);
});

// Ejemplo de consulta a la base de datos
connection.query('SELECT * FROM users', (error, results, fields) => {
    if (error) throw error;
    console.log('Los resultados de la consulta son: ', results);
});

// Cerrar la conexión
connection.end();
```
Pero al ejecutarlo nos da el siguiente error porque el módulo de mysql no está importado:
![[Pasted image 20240705110522.png]]
Por lo que para instalar el módulo, usamos npm:
```javascript
npm install mysql
```
![[Pasted image 20240705110624.png]]
Y ahora ya podremos ejecutar la consulta a la base de datos:
![[Pasted image 20240705110741.png]]
