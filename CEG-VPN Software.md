# VPN Client
You will need to install a VPN client onto your laptop, PC or Mac that can handle OpenVPN
protocols. We recommend:

**Windows**
- OpenVPN GUI Community 64-bit: https://openvpn.net/community (lightweight  open-source app)
**Windows / Mac / Linux**
- OpenVPN Connect: https://openvpn.net/client/  (more resource heavy freeware app).
**Mac**
- Tunnelblick: https://tunnelblick.net/downloads.html

Adding an ovpn file to a client requires Admin rights to the computer.  
**QMUL**  laptops should be setup to allow ovpn file to install.  If you have any problems, reinstall the OpenVPN client from the Software Center to ensure you have the latest configuration.  If this does not work, connect Kelvin. 

Disconnect from any other VPN services you may be using, eg Cisco AnyConnect (EMIS
web) before installing the VPN client. If you are reinstalling, it is best to uninstall the
previous version first. It may help to also reboot your laptop or PC after the install.

You need to have a VPN client (preferable OpenVPN GUI, OpenVPN Connect or Tunnelblick) installed on your laptop or Mac using the latest version of OpenVPN (2.6).  Tunnelblick and OpenVPN Connect should automatically keep themselves up to date.  

The OpenVPN GUI app does not have an automated update facility.  If it is installed on an unmanaged laptop or PC, users should regularly check for updates and re-install if needed.
### Information
OpenVPN GUI: https://community.openvpn.net/openvpn/wiki/OpenVPN-GUI-New
OpenVPN Connect: https://openvpn.net/client/client-connect-vpn-for-windows/
Tunnelblick: https://tunnelblick.net/czQuick.html
# OTP Authenticator
The CEG-VPN now uses TOTP for multi-factor authentication.  You can use any TOTP authenticator phone app compatible with Google Authenticator, including Google Authenticator, Microsoft Authenticator, Duo Mobile, Authy, or 2FAS Auth.  All are available in the appropriate app store.

Where it is provided, app unlock security such as PIN or biometrics, should be used.  Screenshots inside the app should be switched off.  Care should be taken about the storage location for any backup facility. 

