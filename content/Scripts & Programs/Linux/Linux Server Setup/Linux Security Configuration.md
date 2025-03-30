# Linux Security
## Initiate ufw firewall
```bash
sudo ufw app list
```
Output should list OpenSSH
```bash
sudo ufw allow OpenSSH
sudo ufw enable
y enter
sudo ufw status
```
Output should show status as active and list OpenSSH
Firewall now blocks all connections except SSH.
https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04

## Install Support Tools
```bash
sudo apt update
sudo apt install net-tools
sudo apt install build-essential
```

## Fail2ban - SSH Jail
```bash
sudo apt-get install fail2ban
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
'*** ignore first commented [DEFAULT]. amend ~>
[DEFAULT]
bantime = 60m

*** scroll down to # JAILS and add ~>
[sshd]
enabled  = true
port    = ssh
logpath = %(sshd_log)s
'
sudo systemctl start fail2ban
sudo systemctl status fail2ban
```
Creates a jail.local file which will override jail.conf.  Scroll down to JAILS and add/ensure sshd jail is enabled. 
https://www.a2hosting.co.uk/kb/security/hardening-a-server-with-fail2ban

## SSH Hardening
### Restrict Diffie-Hellman
Remove option to use smaller Diffie-Hellman moduli in de-encryption. As root
```bash
sudo su
awk '$5 >= 3071' /etc/ssh/moduli > /etc/ssh/moduli.tmp && mv /etc/ssh/moduli.tmp /etc/ssh/moduli
exit
```
### Restrict Kex, Ciphers and MAC algorithms
Creates a ssh conf file with restrictions that is called by ssh_config. As root
```bash
sudo su
echo -e "\n# Restrict key exchange, cipher, and MAC algorithms, as per sshaudit.com\n# hardening guide.\nKexAlgorithms sntrup761x25519-sha512@openssh.com,curve25519-sha256,curve25519-sha256@libssh.org,gss-curve25519-sha256-,diffie-hellman-group16-sha512,gss-group16-sha512-,diffie-hellman-group18-sha512,diffie-hellman-group-exchange-sha256\nCiphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr\nMACs hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,umac-128-etm@openssh.com\nHostKeyAlgorithms ssh-ed25519,ssh-ed25519-cert-v01@openssh.com,sk-ssh-ed25519@openssh.com,sk-ssh-ed25519-cert-v01@openssh.com,rsa-sha2-512,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256,rsa-sha2-256-cert-v01@openssh.com" > /etc/ssh/sshd_config.d/ssh-audit_hardening.conf
exit
```
### Edit SSH Config
Take backup and open sshd_config for editing
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo nano /etc/ssh/sshd_config
```
#### SSH Protocol
Ensure SSH uses Protocol 2
```
Protocol 2
Include /etc/ssh/sshd_config.d/*.conf
```
#### HostKey order
Supported HostKey algorithms by order of preference
```
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
```
#### Logging
Uncomment to enable
```
SyslogFacility AUTH
LogLevel INFO
```
#### Authentication
```
LoginGraceTime 20
PermitRootLogin prohibit-password
StrictModes yes

PubkeyAuthentication yes

HostbasedAuthentication no
IgnoreRhosts yes

PasswordAuthentication no
PermitEmptyPasswords no
```
RSAAuthentication is deprecated, so if shown, comment out.
#### Authentication 2
```
KerberosAuthentication no
GSSAPIAuthentication no

UsePAM yes
X11Forwarding no
PrintMotd no
```
#### Reduce Timeout Intervals
```
ClientAliveInterval 300
ClientAliveCountMax 3
```
#### Local variables and Subsystems
```
AcceptEnv LANG LC_*
Subsystem      sftp    /usr/lib/openssh/sftp-server
```
### Test SSH
Test SSH configuration - it will return nothing if everything is correct.
Restart SSH
```
sudo sshd -t
sudo systemctl restart ssh
```
Start a new server session **without closing the current session** to check that SSH access is still available. Check that `sudo` and `sudo su` is still available.
https://www.sshaudit.com/hardening_guides.html
https://8gwifi.org/docs/ssh.jsp
https://linuxhint.com/secure-ssh-server-ubuntu/
https://www.linuxbabe.com/security/harden-ssh-server
https://www.digitalocean.com/community/tutorials/how-to-harden-openssh-on-ubuntu-18-04
https://man.openbsd.org/sshd_config

## Secure shared memory
Temp memory used to exchange data between programs can be exploited so restrict it to Read Only. Copy command into fstab.
```bash
sudo nano /etc/fstab
'
tmpfs /run/shm tmpfs defaults,noexec,nosuid	0 0
'
```
https://www.techrepublic.com/article/how-to-enable-secure-shared-memory-on-ubuntu-server/
