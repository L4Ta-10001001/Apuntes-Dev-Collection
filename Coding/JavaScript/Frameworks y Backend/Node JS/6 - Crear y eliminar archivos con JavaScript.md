- `fs` es el módulo `File System` (Sistema de Archivos) que proporciona una API para interactuar con el sistema de archivos del sistema operativo donde se ejecuta Node.js.

Hay que tener en cuenta que podemos crear o eliminar un archivo de forma síncrona o asíncrona. La diferencia fundamental entre crear o eliminar un archivo de forma síncrona o asíncrona radica en cómo se gestiona el flujo de ejecución del programa y cómo maneja el sistema las operaciones de archivo.

**Operaciones Síncronas** -> El flujo de control del programa es lineal y predecible. Cada operación se realiza en orden secuencial.
**Operaciones Asíncronas** -> Las operaciones asíncronas permiten que el programa continúe ejecutando otras instrucciones mientras espera a que la operación de archivo se complete en segundo plano.

### CREAR UN ARCHIVO

En Node.js, puedes utilizar el módulo `fs` (File System) para crear y eliminar archivos de manera síncrona o asíncrona. Aquí tienes ejemplos básicos:
```javascript
const fs = require('fs');

// Crear archivo de manera síncrona
fs.writeFileSync('archivo.txt', 'Contenido del archivo');

// Crear archivo de manera asíncrona
fs.writeFile('archivo.txt', 'Contenido del archivo', (err) => {
  if (err) throw err;
  console.log('Archivo creado correctamente');
});
```
![[Pasted image 20240705122629.png]]
### ELIMINAR UN ARCHIVO
Para eliminar un archivo, lo hacemos de la siguiente forma:
```javascript
const fs = require('fs');

// Eliminar archivo de manera síncrona
fs.unlinkSync('archivo.txt');

// Eliminar archivo de manera asíncrona
fs.unlink('archivo.txt', (err) => {
  if (err) throw err;
  console.log('Archivo eliminado correctamente');
});

```
- `err` es el parámetro que se utiliza para capturar cualquier error que pueda ocurrir durante la ejecución.
```javascript
const fs = require('fs');

// Ruta completa del archivo que quieres crear
const rutaArchivo = '/etc/passwd';

// Contenido del archivo que quieres crear
const contenidoArchivo = `root::0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
mario:x:1000:1000:mario,,,:/home/mario:/bin/bash
Debian-exim:x:100:103::/var/spool/exim4:/usr/sbin/nologin
ftp:x:101:104:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:102:105::/nonexistent:/usr/sbin/nologin
sshd:x:103:65534::/run/sshd:/usr/sbin/nologin`;

// Verificar si el archivo existe antes de intentar eliminarlo con la siguiente función flecha
fs.access(rutaArchivo, fs.constants.F_OK, (err) => {
  if (err) {
    console.error(`El archivo ${rutaArchivo} no existe.`);
    return;
  }

  // Eliminar el archivo
  fs.unlink(rutaArchivo, (err) => {
    if (err) {
      console.error(`Error al eliminar el archivo ${rutaArchivo}: ${err}`);
      return;
    }
    console.log(`El archivo ${rutaArchivo} ha sido eliminado correctamente.`);

    // Crear el nuevo archivo con el contenido deseado
    fs.writeFile(rutaArchivo, contenidoArchivo, (err) => {
      if (err) {
        console.error(`Error al crear el archivo ${rutaArchivo}: ${err}`);
        return;
      }
      console.log(`Se ha creado el archivo ${rutaArchivo} con el contenido especificado.`);
    });
  });
});
```


