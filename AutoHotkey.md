         

Some extra and more detailed info on installing the AutoHotkey programme and script. This enables you to enter codes into EMIS from a csv file.

There are 2 elements:

1.  The AutoHotkey software, which enables you to run automated instructions on your PC, like macros in Excel.
2.  The script containing the AHK instructions for copying the codes – written by Brandon from Primary Care IT

Download the AutoHotkey software [https://www.autohotkey.com/](https://www.autohotkey.com/) and install.

**How to install AutoHotkey on a managed PC / Laptop**

The QMUL managed PCs and laptops will not let you install software to the standard _C:\Program Files\_ folder. However, it is possible to install the software so it runs from your own user folders. The _AppData\Local_ folder in particular isn’t restricted and is used by some programs for single user installs, so it’s a legit programs folder.

-   Click on the install file. 
-   In the install dialogue choose the ‘custom’ option
-   Change the install location from _C:\Program Files\_ to _C:\Users\<user name>\AppData\Local\AutoHotkeys_.
-   Click Next and continue with the install process.

Save the _Code adding script.zip_ file to your computer and unzip it into a suitable location.

You will have two files: _codes.csv_ and _EMIS Code Paster v2.ahk_

-   Open the _EMIS Code Paster v2.ahk_ file using a text editor, like Notepad.
-   Change C:\Users\BrandonCrisp\Desktop\scripts\codes.csv to the path of the _codes.csv_ file on your PC.
-   Save and close your text editor.

Copy and paste the codes to be entered into the codes.csv file and save.

-   Open EMIS and go to the code entry screen for a search.
-   Double click the _EMIS Code Paster v2.ahk_ file to activate it
-   To start the macro/script press CTRL + H
-   To end it press ESC or let it finish going through the code list

Guide: [https://www.autohotkey.com/docs/AutoHotkey.htm](https://www.autohotkey.com/docs/AutoHotkey.htm)

```
#NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
; #Warn  ; Enable warnings to assist with detecting common errors.
SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.

^H:: ; Hotkey to trigger macro is CTRL + H
Loop, read, C:\Users\BrandonCrisp\Desktop\scripts\codes.csv ; codes csv file driectory
{
    LineNumber := A_Index
    Loop, parse, A_LoopReadLine, CSV
    {   
      sleep, 300
	  Send, %A_LoopField%
	  sleep, 1000
	  Send, {Tab}
	  Send, {Tab}
	  Send, {Tab}
	  Send, {Enter}
	  sleep, 800
	  send, ^a
	  sleep, 800
}
}

Escape::
ExitApp
Return
```
