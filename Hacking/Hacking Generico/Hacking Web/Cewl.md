CEWL (Custom Word List generator) es una herramienta en Ruby que se utiliza para generar listas de palabras (wordlists) a partir del contenido de un sitio web. Estas listas son útiles para pruebas de seguridad, especialmente para ataques de fuerza bruta en aplicaciones web.

----

Una vez instalado, puedes usar CEWL desde la línea de comandos. Aquí están algunos comandos básicos:

**Generar una lista de palabras a partir de un sitio web:**
```bash
cewl http://www.ejemplo.com
```
**Guardar la lista de palabras en un archivo:**
```bash
cewl http://www.ejemplo.com -w lista_palabras.txt
```
**Especificar la profundidad de rastreo:**
Por defecto, CEWL solo rastrea la página principal. Puedes especificar la profundidad del rastreo (número de enlaces a seguir) con `-d`:
```java
cewl http://www.ejemplo.com -d 2
```
**Incluir información de metadatos:**
Para extraer palabras de los metadatos del sitio (como títulos de página, descripciones, etc.), usa `-m`:
```bash
cewl http://www.ejemplo.com -m
```
**Especificar el número mínimo de caracteres para las palabras:**
Puedes establecer el número mínimo de caracteres para las palabras incluidas en la lista con `-m`:
```bash
cewl http://www.ejemplo.com -m 5
```
**Utilizar autenticación HTTP básica:**
Aquí hay un ejemplo completo donde se genera una lista de palabras a partir de un sitio web con una profundidad de rastreo de 2, guardando la lista en un archivo y excluyendo palabras de menos de 5 caracteres:
```bash
cewl http://www.ejemplo.com -d 2 -w lista_palabras.txt -m 5
```
