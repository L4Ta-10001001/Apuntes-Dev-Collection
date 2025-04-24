Es posible que queramos destringir el acceso a algún archivo que contenga información delicada, por ejemplo el archivo secreto.txt de mi servidor:
![[Pasted image 20240909171916.png]]
![[Pasted image 20240909171923.png]]
Para conseguir que nadie pueda ver este archivo, tenemos que añadir las siguientes líneas dentro del archivo de configuración de default.conf:
```bash
<Files "/var/www/html/public/secreto.txt">
    Require all denied
</Files>
```
![[Pasted image 20240909172246.png]]
Y ahora no tenemos acceso a dicho archivo:
![[Pasted image 20240909172327.png]]
