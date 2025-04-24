Por ejemplo vamos a crear una base de datos y luego creamos una colección llamada vídeos:
![[Pasted image 20230602095149.png]]
Y aquí creamos otra colección y ya tenemos 2:
![[Pasted image 20230602095219.png]]
### COMO ELIMINAR UNA COLECCIÓN
Para eliminar una colección, usamos db.suscriptores.drop()
```sql
db.suscriptores.drop()
```
Y ahora vemos que ya no existe la colección suscriptores:
![[Pasted image 20230602095428.png]]
# LOS DOCUMENTOS
Los documentos son la forma de enviar y guardar los datos en mongoDB, los cuales van en formato json. Por ejemplo vamos a almacenar estos datos:
```sql
{
  "nombre": "Juan Pérez",
  "edad": 30,
  "ciudad": "Ciudad de México",
  "intereses": ["programación", "viajes", "fotografía"],
  "educacion": {
    "titulo": "Ingeniero en Sistemas",
    "universidad": "Universidad Nacional Autónoma de México"
  }
}
```
Podemos guardar esta data en json de la siguiente forma:
```sql
db.micoleccion.insert({
  "nombre": "Juan Pérez",
  "edad": 30,
  "ciudad": "Ciudad de México",
  "intereses": ["programación", "viajes", "fotografía"],
  "educacion": {
    "titulo": "Ingeniero en Sistemas",
    "universidad": "Universidad Nacional Autónoma de México"
  }
})
```
![[Pasted image 20230602095946.png]]
### CONSULTAR DATOS DE UNA COLECCIÓN
Para ver los datos que hemos añadido anteriormente en la colección vídeos, lo hacemos de esta forma:
![[Pasted image 20230602100110.png]]
También podemos insertar un dato aislado de la siguiente forma:
![[Pasted image 20230602100703.png]]
### DIFERENCIAS ENTRE FIND() Y FINDONE()
La diferencia entre find() y findone() es que find() nos devuelve todas las coincidencias, mientras que findOne() nos devuelve solo la primera coincidencia:
![[Pasted image 20240514191248.png]]
**Insertar un dato que tenga más de un atributo**
```sql
db.micoleccion.insert({"titulo": "Cómo Aprender Mongo", "precio": 24.30})
```
![[Pasted image 20240514185317.png]]
Lo interesante es que la estructura de esta información es diferente, no tiene estructura tal y como vemos, ya que podemos definir la misma estructura desde el lenguaje de programación si así lo preferimos:
**Object Id**
Como vemos, cada vez que insertamos un dato, va acompañado de un ObjectId:
![[Pasted image 20240514184320.png]]
Esto se debe a que cada documento almacenado en una colección tiene un identificador único llamado id. En MongoDB, el `_id` es un campo especial que identifica de manera única cada documento en una colección. Cada documento dentro de una colección tiene su propio `_id`, lo que lo hace único en el contexto de esa colección.
### DOCUMENTOS EN MONGODB
En MongoDB, un documento es un registro individual almacenado en una colección. Los documentos son la unidad básica de almacenamiento de datos en MongoDB y están representados en formato JSON (JavaScript Object Notation). Cada documento consiste en un conjunto de pares de clave-valor que representan los campos y sus valores asociados. Por ejemplo, en este caso tenemos 2 documentos:
![[Pasted image 20240514184546.png]]
### ELIMINAR UN DOCUMENTO DE UNA COLECCIÓN
Para eliminar un solo dato de una colección, lo hacemos de la siguiente forma:
```sql
db.videos.deleteMany({ nombre: 'Juan Pérez' })
```
![[Pasted image 20230602101940.png]]
Y ahora toda la data de este nombre se habrá eliminado:
![[Pasted image 20240514184817.png]]
# FILTRAR UN DATO EN CONCRETO
También podemos obtener únicamente todos los datos de un titulo en específico, por ejemplo del campo nombre o título:
```sql
db.micoleccion.distinct("titulo")
```
![[Pasted image 20240514185515.png]]
