Lo primero será importar el archivo styles.css dentro del main.tsx:
![[Pasted image 20240707235442.png]]
Y a su vez este styles.css se aplica sobre todo nuestro proyecto que está en index.html:
![[Pasted image 20240707235531.png]]
Por ejemplo, si creamos los siguientes estilos sobre el body, se aplicará a la web:
![[Pasted image 20240707235624.png]]
![[Pasted image 20240707235631.png]]
### ESTILOS CSS PARA UN COMPONENTE EXCLUSIVAMENTE
Podemos también conseguir que el estilo CSS sólo se aplique a un componente y no a todo el código, y eso lo hacemos creando un archivo CSS, por ejemplo llamado PrimerComponente.css, y ahí definimos los estilos:
![[Pasted image 20240707235943.png]]
Y a continuación dentro de PrimerComponente.tsx podremos importar este .css:
![[Pasted image 20240708000019.png]]
Y veremos que de esta otra forma hemos aplicado también los estilos:
![[Pasted image 20240708000037.png]]
También se puede crear un directorio llamado styles y ahí guardar todos los estilos:
![[Pasted image 20240708000259.png]]
