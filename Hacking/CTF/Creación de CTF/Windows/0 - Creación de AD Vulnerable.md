## Para crear usuarios
```powershell
New-LocalUser -Name "mario" -Password (ConvertTo-SecureString "Estrella1" -AsPlainText -Force) -PasswordNeverExpires:$true
```
![[Pasted image 20240512160656.png]]
Comprobamos que se haya creado:
![[Pasted image 20240512161826.png]]
Si queremos cambiar la contraseña de un usuario, por ejemplo del administrador, lo haríamos así:
```powershell
Set-LocalUser -Name "Administrador" -Password (ConvertTo-SecureString "Estrella1" -AsPlainText -Force)
```
![[Pasted image 20240513154932.png]]

## Habilitar WINRM
Usamos el siguiente comando:
```powershell
Enable-PSRemoting -Force
```
![[Pasted image 20240512160810.png]]
Vamos a añadir a este usuario Mario dentro del grupo de administradores locales:
```bash
Add-LocalGroupMember -Group "Administradores" -Member "mario"
```
![[Pasted image 20240512161515.png]]
Y ahora ejecutamos el siguiente comando para permitir a usuario locales iniciar sesión por winrm:
```powershell
WinRM quickconfig
```
![[Pasted image 20240512162206.png]]
De tal forma que ya podremos entrar por winrm:
```bash
evil-winrm -i 192.168.0.36 -u 'mario' -p 'Estrella1'
```
![[Pasted image 20240512162231.png]]
## Habilitar RDP
En el panel de control, debemos ir a donde pone Configuiración de Acceso remoto:
![[Pasted image 20240513112316.png]]
Y activar la opción de permitir las conexiones remotas a este equipo:
![[Pasted image 20240513112339.png]]
Pudiendo configurar los usuarios que pueden entrar:
![[Pasted image 20240513112358.png]]
De esta forma, podremos conectarnos por RDP usando xfreerdp:
```bash
xfreerdp /u:mario /p:Estrella1 /v:192.168.0.36:3389
```
![[Pasted image 20240513112437.png]]
## Habilitar SMB1
Lo primero será detectarlo con el siguiente comando:
```powershell
Get-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```
![[Pasted image 20240513113215.png]]
Y ahora lo activamos:
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```
![[Pasted image 20240513113332.png]]
Y ahora ya podemos autenticarnos:
```bash
smbmap -H 192.168.0.36 -u 'mario' -p 'Estrella1'
```
![[Pasted image 20240513113558.png]]


## CREAR SERVIDOR WEB IIS
Habilitamos roles de windows y buscamos IIS:
![[Pasted image 20240513114223.png]]
Y se nos muestran las características del servidor web:
![[Pasted image 20240513114302.png]]
Y ahora por aquí ya tenemos IIS habilitado:
![[Pasted image 20240513114724.png]]
Si vamos al localhost, ya lo tenemos funcionando:
![[Pasted image 20240513114801.png]]
La dirección web se encuentra dentro de esta ruta:
![[Pasted image 20240513114904.png]]
Si ahora queremos añadir alguna webshell en aspx por ejemplo, debemos habilitarlo desde el servidor:
![[Pasted image 20240513120036.png]]
Y ahora vamos a Tipos MIME:
![[Pasted image 20240513120057.png]]
Hacemos doble clic y vamos a donde dice agregar:
![[Pasted image 20240513120120.png]]
Y añadimos esto:
![[Pasted image 20240513120402.png]]
Ahora tenemos que habilitar ASP.NET:
![[Pasted image 20240513121003.png]]
Lo instalamos:
![[Pasted image 20240513121036.png]]
Y ya lo interpreta:
![[Pasted image 20240513121357.png]]
