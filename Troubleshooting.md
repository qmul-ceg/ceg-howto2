# Troubleshooting

## VPN client doesn’t connect
- Check the username and password are correct - be careful not to add spaces, capitalisation etc
- Check the OTP code hasn't regenerated in the time taken to enter it
- If you’ve recently been sent a new ovpn file, make sure you have installed it.
- Check the OpenVPN log for ERROR messages

## OTP doesn't seem to work
- Sometimes the time synchronisation can be a little out.
	- Delete and reinstall the profile in your app.  
	- Try setting up the profile in a different app.

## View & Save VPN client logs

### OpenVPN GUI
- Right-click on the tray icon and select *View Log*
- The log file will open in a text editor
	- *Save as* a new txt file.

### OpenVPN Connect
- In the Profiles view, click on the 'code' icon in the top right-hand corner
- The log file will be displayed
	- Click on the 'share' icon in the top right-hand corner
	- Save the log file to a suitable location

### Tunnelblick
- Click on the Tunnelblick icon in the menu bar, then click "VPN Details…".
- Click on the large "Configurations" button at the top of the window.
- Select the OpenVPN profile on the left side of the window.
- Click on *Copy Console Log to Clipboard* and paste into a txt file.

