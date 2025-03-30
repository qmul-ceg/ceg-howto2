# Linux Maintenance 
## Setup log rotation
```
sudo nano /etc/logrotate.d/rsyslog 
	#
	/var/log/openvpn/openvpn.log {
		daily
		rotate 12
		compress
		copytruncate
		delaycompress
		missingok
		notifempty
	}
```
This sets up openvpn.log to rotate as 12 daily backups

## Install glances
Monitoring overview of server performance
```bash
wget -O- https://bit.ly/glances | /bin/bash
```
https://nicolargo.github.io/glances/

## Install .NET
SDK includes Runtime. Instructions here:
https://docs.microsoft.com/en-gb/dotnet/core/install/linux-ubuntu

## Install net-tools
Provides netstat, ifconfig, route and more.
```bash
sudo apt install net-tools
```

## Install build-essential
https://pimylifeup.com/ubuntu-build-essential/
On Ubuntu, this meta-package includes five individual packages that are crucial to compiling software.
-   `gcc` – This tool is the GNU compiler for the C Programming language.
-   `g++` – This package is the GNU compiler for the C++ programming language.
-   `libc6-dev` – This is the GNU C library. This package contains the development libraries and header files used to compile simple C and C++ scripts.
-   `make` – This is a useful utility that is used for directing the compilation of programs. The make tool interprets a file called a “`makefile`” that directs the compiler how to work.
-   `dpkg-dev` – We can use this package to unpack, build and upload Debian source packages. This utility is useful if you want to package your software for Debian based system.


### Cockpit Setup
```bash
sudo apt-get install cockpit
sudo systemctl enable cockpit.socket
sudo systemctl start cockpit.socket
sudo ufw allow 9090
```
Installs, starts running and opens port for connection. 
Connect on `http://hostname-or-ip-address:9090`