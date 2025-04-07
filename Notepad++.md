# Notepad++
Notepad++ is, as the name suggests, an improved version of the standard Windows Notepad text editor.  It has tabs, for opening multiple files, syntax higlighting for many languages, and many other features.

# Notepad++ Plugin Management
**Note: Notepad++ plugins are not part of the standard QMUL software provision and the IT helpdesk will probably not be able to help with queries or issues.**

The functionality of Notepad++ can be extended by installing plugins.  However, because QM devices have restricted admin rights, you cannot managed plugins via the app’s Plugins menu, as you would with a standard install.  Instead, we need to edit a text file and run a specially written Notepad++ Plugin Management x64 utility (thank you James):

1. Check your _C:\QMUL_ folder.  If it is in lowercase (C:\qmul\’), rename it to uppercase.
2. Check to see if you have a _C:\QMUL\Apps\Notepadpp-x64\pluginlist.txt_ file. If the file or folder doesn't exist...
	1. Find _Notepad++_ in the Software Center and click _Uninstall_
		1. If the Uninstall fails - try
			1. try resyncing the Software Center
			2. check the Forticlient is connected and up-to-date
			3. reboot your laptop
			4. retry several times
			5. contact James and Kelvin via the CPC Profile Teams chat
	2. Once it has completed, click _Install_
	3. Recheck for the folder and file.
3. Open the  _pluginlist.txt_ file in Notepad++. This is a list of all the available Notepad++ plugins (see below)
4. Delete the ‘#’ in front of the plugin(s) that you want to install and Save the file.
5. Close the Notepad++ app.
6. Find _Notepad++ Plugin Management x64_ in the Software Center and click _Install_ or _Repair_, depending on what's showing.
7. Click Yes to confirm and wait… after a few seconds the button turns into _Install_
8. Click _Install_ and wait for the button to revert to _Uninstall._
9. Open the Notepad++ app and check the _Plugins_ Menu for the installed plugin settings

# Notepad++ Plugins
The plugins list is available online:
[https://github.com/notepad-plus-plus/nppPluginList/blob/master/doc/plugin_list_x64.md](https://github.com/notepad-plus-plus/nppPluginList/blob/master/doc/plugin_list_x64.md)
Plugins are created and managed by individuals, so this is a list of the various code repositories (repos).  The list give a brief description of the plugin but following the link to repos will provide some further detail on plugin functionality.  Because these are indiviual projects there can also be some cross-functionality.

# Useful Plugins (for CEG)
(but many others may be of interest and use)
- CSV Lint – display column highlighted csv files + autoscripts for data conversion to SQL
- CSV Query – query csv files using SQL
- NPP QR Code – create QR codes from text
- Random Values – generate passwords
- XML Tools – Pretty Print (ie make readable) XML, plus other tools