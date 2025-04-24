En Docker, hay tres tipos principales de redes predefinidas que puedes utilizar al ejecutar contenedores: bridge, host y none. Aquí hay algunas diferencias clave entre ellas:

1. **Bridge:**
    
    - **Características:** La red bridge es el modo predeterminado en Docker. Cada contenedor se conecta a un puente de red interno en el host.
    - **Comunicación entre contenedores:** Los contenedores en la misma red bridge pueden comunicarse entre sí mediante direcciones IP.
    - **Conexión al host:** Los contenedores en una red bridge pueden comunicarse con el host y con otros contenedores en la misma red bridge, pero están aislados de otras redes bridges en el mismo host.
    - **Conexión a redes externas:** Pueden conectarse a redes externas a través de NAT.

En este tipo de redes, el host podrá conectarse al contenedor:
![[Pasted image 20240302105218.png]]
Y si tenemos 2 contenedores con esta red, también tendrán conexión entre ellos:
![[Pasted image 20240302105309.png]]
![[Pasted image 20240302105321.png]]

1. **Host:**
    
    - **Características:** En el modo host, el contenedor no tiene un espacio de red propio y comparte directamente la interfaz de red del host.
    - **Comunicación entre contenedores:** Los contenedores en modo host pueden comunicarse directamente a través de la interfaz de red del host, sin necesidad de puente de red.
    - **Conexión al host:** Los contenedores en modo host pueden acceder directamente a los servicios que se ejecutan en el host, como si estuvieran instalados localmente.
    - **Conexión a redes externas:** La conectividad es la misma que la del host, ya que comparten la interfaz de red.

```bash
docker run -it --network=host 3d
```
Y veremos que la IP de este contenedor será la misma IP que tendrá mi máquina anfitrión:
![[Pasted image 20240302104830.png]]
![[Pasted image 20240302104847.png]]

1. **None:**
    
    - **Características:** En el modo none, el contenedor no tiene una interfaz de red asignada.
    - **Comunicación entre contenedores:** Los contenedores en modo none están completamente aislados de la red, y no pueden comunicarse directamente entre sí.
    - **Conexión al host:** No hay comunicación directa con la interfaz de red del host.
    - **Conexión a redes externas:** El contenedor en modo none no puede acceder directamente a redes externas.

En este tipo de red si se la asignamos a un contenedor, no tendrá conexión con redes externas:
```bash
docker run -it --network=none 3d
```
![[Pasted image 20240302104651.png]]

En resumen, la elección entre bridge, host y none depende de los requisitos de conectividad y aislamiento de tu aplicación. Bridge es comúnmente utilizado cuando necesitas aislar contenedores pero permitir su comunicación interna. Host es útil cuando deseas que un contenedor comparta directamente la interfaz de red del host. None es útil cuando deseas un alto grado de aislamiento y no necesitas conectividad de red para el contenedor.

------------------

