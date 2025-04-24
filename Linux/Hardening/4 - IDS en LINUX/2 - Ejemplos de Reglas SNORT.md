**Detección de un Escaneo de Puertos**
```bash
alert tcp any any -> any 80 (msg:"Escaneo de puertos en HTTP"; flags:S; threshold:type threshold, track by_src, count 10, seconds 10; sid:1000003; rev:1;)
```

**Detección de Tráfico SSH de Fuerza Bruta**

```bash
alert tcp any any -> any 22 (msg:"Intento de fuerza bruta SSH"; flow:to_server,established; threshold:type threshold, track by_src, count 5, seconds 60; sid:1000016; rev:1;)
```

**Prevención de Ataques de Denegación de Servicio (DoS)**
```bash
alert udp any any -> 192.168.1.100 53 (msg:"Posible ataque DoS en DNS"; threshold:type threshold, track by_src, count 100, seconds 60; sid:1000005; rev:1;)
```



**Detección de Tráfico HTTP Inusual**
```bash
alert http any any -> any any (msg:"Tráfico HTTP inusual"; content:"/admin"; http_uri; sid:1000006; rev:1;)
```




**Filtrado de Tráfico Basado en IP**

```bash
drop ip any any -> 10.0.0.5 any (msg:"Bloqueo de tráfico hacia IP 10.0.0.5"; sid:1000007; rev:1;)
```
