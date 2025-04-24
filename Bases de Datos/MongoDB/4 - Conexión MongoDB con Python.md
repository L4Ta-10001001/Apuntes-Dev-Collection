Usamos la librería pymongo:
```python
from pymongo import MongoClient

client = MongoClient()

db = client.mi_database
collection = db.micoleccion
result = collection.find({})

for document in result:
    print(document)

client.close()
```
![[Pasted image 20240514193231.png]]
