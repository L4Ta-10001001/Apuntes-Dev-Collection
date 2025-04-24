Podemos crear un script en bash para hacer fuzzing web de la siguiente forma:
```bash
#!/bin/bash

# Verifica que se proporcionen dos argumentos: el diccionario y la URL
if [ $# -ne 2 ]; then
    echo "Uso: $0 <diccionario> <URL>"
    exit 1
fi

diccionario="$1"
url="$2"

total_lines=$(wc -l < "$diccionario")
current_line=0

# Verifica si el diccionario existe
if [ ! -f "$diccionario" ]; then
    echo "El diccionario '$diccionario' no existe."
    exit 1
fi

# Itera sobre cada línea en el diccionario
while read -r linea; do

    current_line=$((current_line + 1))
    echo -ne "Progreso: $current_line/$total_lines\r"

    respuesta=$(curl -s -o /dev/null -w "%{http_code}" "$url$linea/")

    # Verifica si el código de respuesta es 200 (OK)
    if [ "$respuesta" -eq 200 ]; then
        echo "URL accesible: $url$linea/"
    fi
done < "$diccionario"
```
E incluso podemos añadir un indicador de progreso en el script:
![[Pasted image 20231206105021.png]]
### OTRO EJEMPLO:
```bash
#!/bin/bash

# Definimos colores para la salida
ROJO='\033[0;31m'
VERDE='\033[0;32m'
AMARILLO='\033[1;33m'
REINICIAR='\033[0m'

# Verifica que el usuario haya proporcionado los argumentos necesarios
if [ "$#" -ne 2 ]; then
    echo -e "${ROJO}Uso incorrecto. Debes proporcionar una URL con el marcador PINGU y un diccionario${REINICIAR}"
    echo -e "Ejemplo: $0 'http://example.com/page?id=PINGU' diccionario.txt"
    exit 1
fi

# Variables
URL=$1
DICCIONARIO=$2

# Verifica si el archivo de diccionario existe
if [ ! -f "$DICCIONARIO" ]; then
    echo -e "${ROJO}El archivo $DICCIONARIO no existe.${REINICIAR}"
    exit 1
fi

# Obtiene el tamaño del contenido de la respuesta original sin el marcador PINGU
PETICION_FILTRADA=$(curl -s -o /dev/null -w "%{size_download}" "${URL//PINGU/}")

# Contar el total de líneas en el archivo de diccionario
TOTAL_LINEAS=$(wc -l < "$DICCIONARIO")
LINEA_ACTUAL=0

# Función para mostrar la barra de progreso
mostrar_progreso() {
    local progreso=$1
    local total=$2
    local porcentaje=$(( (progreso * 100) / total ))
    local llenado=$(( (porcentaje * 40) / 100 ))
    local vacio=$(( 40 - llenado ))
    printf "\r["
    printf "%0.s${VERDE}#${REINICIAR}" $(seq 1 $llenado)
    printf "%0.s${AMARILLO}-${REINICIAR}" $(seq 1 $vacio)
    printf "] %d%%" "$porcentaje"
}

# Imprimir mensaje inicial
echo -e "${AMARILLO}Iniciando el proceso...${REINICIAR}"

# Leer cada línea del archivo de diccionario y reemplazar PINGU en la URL
while IFS= read -r PAYLOAD; do
    URL_FUZZED=${URL//PINGU/$PAYLOAD}

    # Realizar la petición con curl y obtener el tamaño del contenido de la respuesta
    TAMANIO_RESPUESTA=$(curl -s -o /dev/null -w "%{size_download}" "$URL_FUZZED")

    # Si el tamaño de la respuesta es diferente al original y no es 0, imprimir el payload y la URL
    if [ "$TAMANIO_RESPUESTA" -ne "$PETICION_FILTRADA" ] && [ "$TAMANIO_RESPUESTA" -ne 0 ]; then
        echo -e "${VERDE}Payload válido:${REINICIAR} $PAYLOAD -> ${VERDE}Tamaño del contenido:${REINICIAR} $TAMANIO_RESPUESTA"
        echo -e "${VERDE}URL válida:${REINICIAR} $URL_FUZZED
        "
    fi

    # Actualizar el progreso
    LINEA_ACTUAL=$((LINEA_ACTUAL + 1))
    mostrar_progreso $LINEA_ACTUAL $TOTAL_LINEAS
done < "$DICCIONARIO"

# Finalizar la barra de progreso
echo -e "\n${AMARILLO}Completado.${REINICIAR}"
```
