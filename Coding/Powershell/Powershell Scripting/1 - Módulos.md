Para verificar qué módulos están actualmente importados, puedes usar:
```powershell
Get-Module
```
![[Pasted image 20240921103930.png]]
Para importar módulos, primero tenemos que cambiar la política de ejecución a `RemoteSigned` o `Unrestricted`. Para cambiarla, ejecuta PowerShell como administrador y usa uno de los siguientes comandos:
```powershell
Set-ExecutionPolicy RemoteSigned # Para permitir scripts locales no firmados.

Set-ExecutionPolicy Unrestricted # Para permitir todos los scripts (menos seguro).
```
Y luego ya podemos importar un módulo en concreto:
```powershell
Import-Module .\ps2exe.psm1
```
![[Pasted image 20240921104352.png]]
Y ahora podremos ejecutar el módulo correspondiente:
```powershell
Invoke-PS2EXE
```
![[Pasted image 20240921104843.png]]
