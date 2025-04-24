Podemos ejecutar código javascript con node del lado del servidor ejecutando el comando node backend.js:
![[Pasted image 20240730143314.png]]
También podríamos levantar un servidor HTTP con node de la siguiente forma:
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hola, Mundo!\n');
});

const PORT = 3000;
server.listen(PORT, () => {
    console.log(`Servidor corriendo en http://localhost:${PORT}/`);
});
```
![[Pasted image 20240730143527.png]]
![[Pasted image 20240730143537.png]]
