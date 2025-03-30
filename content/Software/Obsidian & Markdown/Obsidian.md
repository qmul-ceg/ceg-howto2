# Multiple Pinned Vaults on Taskbar
- Navigate to each Vault's top folder
- Create a new Shortcut (example is for My Vault)
	- URL = `obsidian://open/?vault=My%20Vault`
		- spaces need to replaced with %20.
	- Name = My Vault (ie name of Vault)
- Open Obsidian 
	- Pin icon to Taskbar
	- Drag each Vault shortcut to pinned icon.
- Right click Taskbar icon to see Vaults listed under Pinned. Select Vault(s)
https://forum.obsidian.md/t/pin-vaults-in-windows-taskbar/16068/2

# Auto-Open Vault Selector
Creates a shortcut or taskbar icon that opens the Obsidian Vault Selector
*Replace “UserName” below with your actual user name.*
- Create a script file named `obsidian.ps1` in central location,eg `c:\Users\UserName`, containing:
```powershell
$filePath = "c:\Users\UserName\AppData\Roaming\obsidian\obsidian.json"
(Get-Content $filePath).Replace("`"open`":true","`"open`":false") | Set-Content $filePath
start obsidian:
```

- Right-click on the script file and click “Create shortcut”. 
	- Rename the shortcut *ObsidianS*
- Right-click on *ObsidianS* and go into “Properties”.
	- In the “Target:” field put:
	`powershell -noLogo -ExecutionPolicy unrestricted -file C:\Users\UserName\Obsidian.ps1`
	- Click the “Change Icon…” button and put:
	`%USERPROFILE%\AppData\Local\Obsidian\Obsidian.exe` as the icon location.
	- Apply and OK

- Move or copy the shortcut file to where required. You can pin it to your taskbar or start menu by right-clicking and selecting appropriate option.

https://forum.obsidian.md/t/option-open-last-used-vault-or-open-vault-selector/7093/20



