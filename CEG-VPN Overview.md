# CEG-VPN Overview
The CEG-VPN (Virtual Private Network) is a secured network hosted in Amazon Web Services, which is accessible only via a dedicated OpenVPN server.  The VPN contains several CEG resources, most notably Compass - our Discovery subscriber database.

Connection to the OpenVPN server requires the user to have a suitable OpenVPN client installed on their laptop or PC.  This establishes a secure tunnel between their device and the OpenVPN server using a series of keys and protocols to encrypt the connections and communications.  The user's encryption configuration is provided in a personal .ovpn file, which the user imports into the VPN client.

The OpenVPN server also requires the user to authenticate with a username, password and TOTP (Time-synchronised One Time PIN).  The user is provided with a QR code that allows them to setup a profile on an appropriate OTP phone app to generate the required TOTP.  The user is requested to further secure the OTP app with any available PIN or biometric security measures.

The encryption used throughout is inline with NHS Digital and National Cyber Security Centre (NCSC) guidelines and employs up to date methods and standards.


