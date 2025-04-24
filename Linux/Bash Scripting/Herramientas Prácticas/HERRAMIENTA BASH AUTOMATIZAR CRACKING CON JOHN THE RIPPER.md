```bash
#!/bin/bash

if [ $# -ne 2 ]; then
    echo "Uso: <archivo> <wordlist>"
    exit 1
fi

input_file=$1
wordlist=$2

# Determinar el formato y generar el hash
case "$input_file" in
    *.kdbx)
        keepass2john "$input_file" > hash
        ;;
    *.rar)
        rar2john "$input_file" > hash
        ;;
    *.zip)
        zip2john "$input_file" > hash
        ;;
    *)
        echo "Formato no compatible"
        exit 1
        ;;
esac

# Intentar crackear la contraseña
john --wordlist="$wordlist" hash

# Mostrar resultados
john --show hash

# Limpieza: Eliminar archivos temporales
rm hash
```