# Linux Performance Tweaks
## Improve Memory Performance
Data kept in memory. Reducing means less data kept improving efficiency. 10 is a good compromise but can be adjusted for need.
```bash
sudo nano /etc/sysctl.conf
'
vm.swappiness = 10
'
```
https://linuxhint.com/understanding_vm_swappiness/
https://www.gwynsoft.com/en/linux/ubuntu/ubuntu-16-04-lts-server/setting-up-an-aws-instance-with-ubuntu-16-04-lts-and-virtualmin-pt2

## Increase open file limit and therefore concurrent users
Elevate to root and echo in parameters.
```bash
sudo -i
echo "* soft nofile 1000000" >> /etc/security/limits.conf
echo "* hard nofile 1000000" >> /etc/security/limits.conf
echo "session required pam_limits.so" >> /etc/pam.d/common-session
su ubuntu
```
http://woshub.com/too-many-open-files-error-linux/