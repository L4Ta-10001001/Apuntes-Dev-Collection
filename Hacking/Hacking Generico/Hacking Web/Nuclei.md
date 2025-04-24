Nuclei es una herramienta poderosa para realizar escaneos de vulnerabilidades y reconocimiento en aplicaciones web. Es altamente personalizable y utiliza plantillas YAML para definir las reglas de escaneo.

Por ejemplo, si buscamos por plantillas de lfi encontramos lo siguiente:
```bash
find /root/.local/nuclei-templates/ -type f -name "*lfi*.yaml"
```
![[Pasted image 20240811104525.png]]

-----

Para comprobar si tenemos nuclei instalado ejecutamos el siguiente comando:
```bash
nuclei --version
```
Para escanear un solo objetivo:
```bash
nuclei -u http://example.com
```
![[Pasted image 20240811102810.png]]
Si quisiéramos escanear más objetivos a la vez, utilizamos el siguiente comando:
```bash
nuclei -l urls.txt
```
O también podemos hacer una búsqueda por una vulnerabilidad en concreto, es decir, utilizar una plantilla en concreto, por ejemplo la CVE-2012-1823.yaml:
```bash
nuclei -u http://192.168.0.29 -t cves/2012/CVE-2012-1823.yaml
```
![[Pasted image 20240811103047.png]]
Podemos actualizar estas plantillas con el siguiente comando:
```bash
nuclei -update-templates
```
Comprobamos que la vulnerabilidad encontrada existe, y así es:
```bash
https://github.com/0xl0k1/CVE-2012-1823
```
![[Pasted image 20240811103349.png]]
Incluso si dejamos más tiempo el escaneo, vemos que nos encuentra más vulnerabilidades:
![[Pasted image 20240811104115.png]]
### ESTABLECER FILTROS
Si queremos que nuclei solo nos reporte vulnerabilidades críticas, podemos aplicar filtros:
```bash
nuclei -u http://example.com -severity high,critical
```
