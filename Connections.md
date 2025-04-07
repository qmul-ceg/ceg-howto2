## User Logins to Server
Current active logins, including IP and encryption, are shown in the openvpn-status.log
```
sudo cat /var/log/openvpn/openvpn-status.log
```
The assigned IPs are listed in ipp.txt.
```
sudo cat /var/log/openvpn/ipp.txt
```

OpenVPN connections are also logged in syslog, each day is archived and kept for 7 days. The archives for days 2-7 previous are gz compressed.  Use `grep` or `zgrep`, searching on user name or "openvpn" - [[./Text & Log Search & Edit#grep tail|Text & Log Search & Edit > grep tail]]

## OpenVPN Connection Attempts Log
OpenVPN's handling of traffic - attempts to connect via openvpn - are logged in openvpn.log
```
sudo tail -50 /var/log/openvpn/openvpn.log
```
Look for errors and fails.
https://openvpn.net/community-resources/reference-manual-for-openvpn-2-4/
https://openvpn.net/community-resources/how-to/#starting-up-the-vpn-and-testing-for-initial-connectivity

## Port Activity
install net-tools
```
sudo netstat -pnta
```
Shows programs listening on ports
https://www.ionos.co.uk/digitalguide/server/tools/introduction-to-netstat/

 Look for openvpn on 0.0.0.0:443
```
sudo tcpdump -i eth0 -nn -s0 -v port 443
```
Will show the traffic arriving on port 443 - attempt to connect via openvpn and look for IP, errors and failure messages
-i : interface. Not required if there is only one network adapter.  
-nn : -n do not resolve hostnames. -nn do not resolve hostnames or ports. 
-s0 : Snap length, size of the packet to capture. `-s0` to capture all the traffic. 
-v : Verbose, using (**-v**) or (**-vv**) 
port 443 : port filter
host 10.10.1.1: IP address (destinationa and source)
https://hackertarget.com/tcpdump-examples/

## SSH Direct Connection
To enable direct SSH and to use of ping, tracert etc
Look up IP address and edit VPN-02 Security Group Inbound to:
>All traffic on < IP>

PuTTY onto `35.178.10.20` port 22
Remove rule from Security Group when finished.

---