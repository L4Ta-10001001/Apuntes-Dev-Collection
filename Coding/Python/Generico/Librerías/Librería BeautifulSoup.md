```python
from bs4 import BeautifulSoup

# HTML de ejemplo
html_content = '''
<html>
<head><title>Ejemplo</title></head>
<body>
<div class="content">Contenido 1</div>
<div class="content">Contenido 2</div>
<div class="footer">Pie de página</div>
</body>
</html>
'''

# Crear el objeto BeautifulSoup
soup = BeautifulSoup(html_content, 'html.parser')

# Encontrar todos los elementos con la clase 'content'
content_divs = soup.find_all('div', class_='content')

# Imprimir el texto de cada elemento
for div in content_divs:
    print(div.text)
```
![[Pasted image 20240609123141.png]]
