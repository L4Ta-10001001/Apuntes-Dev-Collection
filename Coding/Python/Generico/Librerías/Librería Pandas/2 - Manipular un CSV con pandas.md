Tenemos el siguiente csv:
```bash
edad,nombre,trabajo
25,Ana,Ingeniera
30,Carlos,Contador
22,Marta,Programadora
35,Luis,Médico
28,Sofía,Profesora
```
![[Pasted image 20240107153930.png]]
Podemos utilizar el siguiente código para imprimir esta información con Python:
```python
import pandas as pd

# Lee el archivo CSV y lo guarda en la variable df (DataFrame)
df = pd.read_csv('datos.csv')

# Imprime el DataFrame
print(df)
```
Y nos imprime el contenido:
![[Pasted image 20240107152631.png]]
**Seleccionar una determinada columna**
Para imprimir las filas de una determinada columna lo haríamos de esta forma:
```python
import pandas as pd

# Lee el archivo CSV
df = pd.read_csv('datos.csv')

# Seleccionar una determinada columna

columna = df['nombre']

# Imprime el resultado
print(columna)
```
![[Pasted image 20240107193324.png]]
**Seleccionar las primeras filas de un DataFrame**
Lo haríamos con un head:
```python
import pandas as pd

# Cargar un archivo CSV en un DataFrame
df = pd.read_csv('datos.csv')

# Mostrar las primeras filas del DataFrame
print(df.head())
```
![[Pasted image 20240107193509.png]]
**Mostrar aquellas filas que cuya columna empiece por un determinado patrón**
Para imprimir por ejemplo aquellas filas que empiecen por la letra M, lo hacemos con el siguiente código:
```python
import pandas as pd

# Lee el archivo CSV
df = pd.read_csv('datos.csv')

# Creamos el filtro en la variable filtro y luego lo aplicamos.
filtro = df['nombre'].str.startswith('M') # Creamos el filtro
resultados_filtrados = df.loc[filtro] # Aplicamos el filtro

# Imprime el resultado
print(resultados_filtrados)
```
![[Pasted image 20240107152951.png]]
También podemos hacer esto mismo con la fila que queramos de cualquier columna, por ejemplo ahora de la columna trabajo aquellas filas que empiecen por la letra C:
```python
import pandas as pd

# Lee el archivo CSV
df = pd.read_csv('datos.csv')

# Aplica el filtro para la columna "trabajo"
filtro_trabajo = df['trabajo'].str.startswith('C')
resultados_filtrados_trabajo = df.loc[filtro_trabajo]

# Imprime el resultado
print(resultados_filtrados_trabajo)
```
![[Pasted image 20240107153117.png]]
**Filtrar por aquellas filas que contengan una determinada letra**
Podemos hacer que se nos impriman por ejemplo aquellas filas de la columna nombre que contengan la letra a:
```python
import pandas as pd

# Lee el archivo CSV
df = pd.read_csv('datos.csv')

# Aplica el filtro para la columna "nombre" aquellas filas que contengan la letra a
filtro_nombre = df['nombre'].str.contains('a')

resultados_filtrados = df.loc[filtro_nombre]

# Imprime el resultado
print(resultados_filtrados)
```
![[Pasted image 20240107153331.png]]
O también que tengan la letra a y la letra c:
```python
import pandas as pd

# Lee el archivo CSV
df = pd.read_csv('datos.csv')

# Aplica el filtro para la columna "nombre" a aquellas filas que tengan la letra 'a'
filtro_a = df['nombre'].str.contains('a')

# Aplica el filtro para la columna "nombre" a aquellas filas que tengan la letra 'c'
filtro_c = df['nombre'].str.contains('c')

# Combina los filtros utilizando el operador lógico OR (|)
resultados_filtrados = df.loc[filtro_a | filtro_c]

# Imprime el resultado
print(resultados_filtrados)
```
![[Pasted image 20240107153733.png]]
**Hacer una búsqueda de aquellas filas de una columna que acaben de una determinada forma**
Por ejemplo podemos obtener todos los nombres que terminen por la letra s:
```python
import pandas as pd

# Lee el archivo CSV
df = pd.read_csv('datos.csv')

# Aplica el filtro para la columna "nombre" a aquellas filas cuyos nombres terminan con la letra 's'
filtro_s = df['nombre'].str.endswith('s')

# Filtra el DataFrame utilizando el filtro
resultados_filtrados = df.loc[filtro_s]

# Imprime el resultado
print(resultados_filtrados)
```
![[Pasted image 20240107153856.png]]
**Filtrar filas por la edad**
Para poder filtrar aquellas filas que tengan un número inferior o mayor que un determinado patrón, lo hacemos de esta forma:
```python
import pandas as pd

# Lee el archivo CSV
df = pd.read_csv('datos.csv')

# Filtramos el dataframe por la edad

df_filtrado = df[df['edad'] > 30]

# Imprime el resultado
print(df_filtrado)
```
![[Pasted image 20240107193154.png]]
