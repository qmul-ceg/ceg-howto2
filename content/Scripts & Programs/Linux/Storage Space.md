## Disk and Device info
Best method - actual disk space usage
```bash
df -h

Filesystem      Size  Used Avail Use% Mounted on
udev            3.9G     0  3.9G   0% /dev
tmpfs           795M  852K  794M   1% /run
/dev/xvda1       30G  6.9G   23G  24% /
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
tmpfs           3.9G     0  3.9G   0% /run/shm
/dev/loop0       26M   26M     0 100% /snap/amazon-ssm-agent/5656
/dev/loop2      115M  115M     0 100% /snap/core/13741
/dev/loop1      115M  115M     0 100% /snap/core/13886
/dev/loop4       27M   27M     0 100% /snap/amazon-ssm-agent/5163
/dev/loop3       56M   56M     0 100% /snap/core18/2560
/dev/loop6       62M   62M     0 100% /snap/core20/1611
/dev/loop5       56M   56M     0 100% /snap/core18/2566
/dev/loop7       64M   64M     0 100% /snap/core20/1623
/dev/loop9       68M   68M     0 100% /snap/lxd/22753
/dev/loop8       68M   68M     0 100% /snap/lxd/22526
tmpfs           795M     0  795M   0% /run/user/1000
```

Clearer breakdown by partition - block device info
```bash
lsblk

NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0     7:0    0  25.1M  1 loop /snap/amazon-ssm-agent/5656
loop1     7:1    0   115M  1 loop /snap/core/13886
loop2     7:2    0 114.9M  1 loop /snap/core/13741
loop3     7:3    0  55.6M  1 loop /snap/core18/2560
loop4     7:4    0  26.7M  1 loop /snap/amazon-ssm-agent/5163
loop5     7:5    0  55.6M  1 loop /snap/core18/2566
loop6     7:6    0    62M  1 loop /snap/core20/1611
loop7     7:7    0  63.2M  1 loop /snap/core20/1623
loop8     7:8    0  67.9M  1 loop /snap/lxd/22526
loop9     7:9    0  67.8M  1 loop /snap/lxd/22753
xvda    202:0    0    30G  0 disk
└─xvda1 202:1    0    30G  0 part /
xvdb    202:16   0    10G  0 disk
```

Overly detailed disk partition info
```bash
fdisk -l
...
Disk /dev/xvda: 30 GiB, 32212254720 bytes, 62914560 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xdf3095e2

Device     Boot Start      End  Sectors Size Id Type
/dev/xvda1 *     2048 62914526 62912479  30G 83 Linux
...
```
https://linuxhandbook.com/linux-list-disks/

## Folder Size Info
Check space on server using *disk usage* `du`
```
sudo du -had1
```
h = human readable
a = all
d1=folder depth of 1
Provides disk usage of each folder in the current location.

Starting at root, check for folder usage, moving to down the folder levels.

## System Log Files
Main log folder is `/var/log/` and system log is `syslog` with numbered daily backups.  If there's a repeating error, this can grow and cause it's own errors.
#### checking logs
Text search in situ [[Text & Log Search & Edit#grep tail]]
If needed copy file to local drive.  This may require temporarily changing the file permissions.
`chmod 744` should allow copying.  Remember to change it back.
#### clearing logs
Do not `rm` and rebuild as services may fail with no file to write to.
```
sudo cat /dev/null > /path/to/log.file
```
Writes null into the log file.  

## DDS Filer Log Files
dds log files can be deleted when not in use. Check that filer isn't running first.
 ```bash
cd /data/filer_xxx/log
sudo rm *.txt
```
  
To truncate nohup.out file after manual filer run:
```bash
cd /data/filer_xxx   #cd to filer directory
sudo su    #change to root user
>nohup.out   #copy nothing into nohup.out 
```  

## Install and Update Clean Up
The apt cache and old packages can require a cleanout.
```bash
sudo du -sh /var/cache/apt # size of apt cache
sudo apt autoclean #removes obsolete install files from apt cache
sudo apt clean #removes all install files from apt cache

sudo apt autoremove #removes unrequired packages
sudo apt autoremove --purge <package> #removes package and configuration files.
```

Vacuum the systemd journal logs
```bash
journalctl --disk-usage #size of Linux logs
sudo journalctl --vacuum-time=3d #remove logs over 3 days old
```

To delete archived Snap packages. Script is here: [https://itsfoss.com/free-up-space-ubuntu-linux/](https://itsfoss.com/free-up-space-ubuntu-linux/) 
```bash
du -h /var/lib/snapd/snaps`  #size of Snap packages. 
```