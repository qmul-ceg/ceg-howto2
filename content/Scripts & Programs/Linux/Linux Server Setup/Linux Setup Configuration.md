# Linux Setup
## Upgrade Services
```bash
sudo apt update && sudo apt upgrade
```

## Stop Service Restart Prompt
New in Ubuntu 22.04 - needrestart default set to "interactive" mode which causes apt update to prompt - `Daemons using outdated libraries - Which service should be restarted?`
Edit needrestart conf file and reboot.
```bash
sudo nano /etc/needrestart/needrestart.conf
'
AMEND
#$nrconf{restart} = 'i';
TO
$nrconf{restart} = 'a';
'
sudo shutdown -r now
```
https://stackoverflow.com/questions/73397110/how-to-stop-ubuntu-pop-up-daemons-using-outdated-libraries-when-using-apt-to-i










