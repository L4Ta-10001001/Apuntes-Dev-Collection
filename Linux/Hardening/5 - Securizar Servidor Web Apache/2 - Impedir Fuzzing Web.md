Puedes usar el módulo **mod_evasive** para evitar ataques de fuerza bruta o fuzzing enviando un número excesivo de solicitudes en poco tiempo.
```bash
sudo apt install libapache2-mod-evasive
```
Y ahora editaremos el archivo `/etc/apache2/mods-available/evasive.conf` donde añadiremos las siguientes líneas:
```bash
<IfModule mod_evasive20.c>
    DOSHashTableSize    3097
    DOSPageCount        2
    DOSSiteCount        50
    DOSPageInterval     1
    DOSSiteInterval     1
    DOSBlockingPeriod   10
    DOSEmailNotify      youremail@domain.com
    DOSSystemCommand    "sudo /sbin/iptables -A INPUT -s %s -j DROP"
</IfModule>
```
![[Pasted image 20240909172603.png]]
- `DOSPageCount`: Número de accesos a la misma página en un intervalo de tiempo que desencadenará la protección.
- `DOSSiteCount`: Número total de solicitudes al sitio en un intervalo de tiempo que desencadenará la protección.
- `DOSBlockingPeriod`: Tiempo (en segundos) que un cliente será bloqueado después de sobrepasar el límite.
- `DOSSystemCommand`: Ejecuta un comando al detectar abuso, en este caso bloquea al atacante en el firewall con `iptables`.

Ahora, si quisiéramos ver los módulos instalados, lo haríamos de la siguiente forma:
```bash
apache2ctl -M | grep evasive
```
![[Pasted image 20240909172845.png]]
De tal forma que si nosotros ahora hacemos fuzzing web, nuestra IP estará bloqueada:
![[Pasted image 20240909172936.png]]
![[Pasted image 20240909172945.png]]
También podemos comprobar el correcto funcionamiento revisando los logs de apache `sudo tail -f /var/log/apache2/error.log` donde veremos que el módulo ha funcionado bien:
![[Pasted image 20240909173506.png]]
