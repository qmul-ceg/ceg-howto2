Check current logins before shutdown [[Script Guides & Snippets/Linux/Connections#View Connections]]

## shutdown
```
sudo shutdown <-rhP> <-t sec> <time>
```
*flag* =
-r Reboot
-h Shutdown and Halt (shutdown CPU)
-P Shutdown and Powerdown (cuts power)
*wait* =
-t wait sec seconds before shutdown
*time* = 'now' or +m minutes