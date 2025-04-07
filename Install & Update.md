### update and upgrade packages 
```bash
sudo apt install <package name>

sudo apt update #updates the package list
sudo apt upgrade #installs latest version
```
OR
```bash
sudo apt update && sudo apt upgrade
```

### remove unneeded packages and old logs
```bash
sudo apt autoremove
sudo apt autoclean
```
```bash
journalctl --disk-usage
sudo journalctl --vacuum-time=3d
```


