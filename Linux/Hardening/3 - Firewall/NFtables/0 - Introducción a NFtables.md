El **firewall Nftables** es un sistema que permite gestionar reglas de filtrado de paquetes en Linux, reemplazando a iptables. Proporciona una interfaz unificada para diferentes tipos de filtrado (IPv4, IPv6, ARP, bridge, etc.) y es más eficiente que sus predecesores.

### **Arquitectura de Nftables**

- **Tablas**: Contienen cadenas (chains) y están asociadas a familias de protocolos (inet para IPv4 e IPv6, arp, bridge).
- **Cadenas (Chains)**: Dentro de las tablas, las cadenas especifican el flujo de tráfico (como `input`, `forward`, `output`). Las reglas de filtrado se aplican dentro de estas cadenas.
- **Reglas (Rules)**: Las reglas definen acciones a realizar sobre los paquetes que coinciden con ciertos criterios (como aceptar o rechazar).

-----------

Para crear una regla que por ejemplo impida el tráfico ICMP, debemos de crear primero una tabla y una cadena:
```bash
# Crear una tabla para el firewall
nft add table inet firewall

# Crear una cadena para los paquetes entrantes (input)
nft add chain inet firewall input { type filter hook input priority 0 \; }

# Crear una cadena para los paquetes salientes (output)
nft add chain inet firewall output { type filter hook output priority 0 \; }
```
![[Pasted image 20240907101012.png]]
Ahora podemos consultar la tabla creada:
```bash
nft list tables
```
![[Pasted image 20240907102310.png]]
Y ahora ya podremos crear las reglas que bloqueen el tráfico ICMP, de tal forma que se descarta cualquier paquete ICMP tanto de entrada como de salida:
```bash
# Bloquear el tráfico ICMP entrante
nft add rule inet firewall input ip protocol icmp drop

# Bloquear el tráfico ICMP saliente
nft add rule inet firewall output ip protocol icmp drop
```
![[Pasted image 20240907101034.png]]
Si queremos verificar las reglas existentes, ejecutamos el siguiente comando:
```bash
nft list table inet firewall
```
![[Pasted image 20240907101237.png]]
En este punto, si yo intento hacer un ping a esta máquina, no voy a poder:
![[Pasted image 20240907101147.png]]
#### ELIMINAR REGLAS
Para eliminare reglas, primero debemos de detectar el handle_number de dicha regla, con los siguientes comandos:
```bash
nft -a list chain inet firewall input 3
nft -a list chain inet firewall output 4
```
![[Pasted image 20240907101604.png]]
#### ELIMINAR CADENAS
De todos modos, una vez eliminadas las reglas, debemos tener en cuenta que habrán quedado las cadenas, que también podremos eliminarlas con el mismo comando:
```bash
nft delete chain inet firewall input
nft delete chain inet firewall output
```
![[Pasted image 20240907102021.png]]
#### ELIMINAR TABLA
Si después de eliminar las reglas y las cadenas quieres eliminar toda la tabla, también es posible. Recuerda que para eliminar una tabla, primero debes haber eliminado todas las cadenas dentro de ella:
```bash
nft delete table inet firewall
```
![[Pasted image 20240907102340.png]]
