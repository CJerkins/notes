This readme is for new users to follow these critical taskes. New users need to setup the kit for them with the tech when setting up for the first time.

all default passwords are drokDROK63
## Laptop setup
* * *
### OS setup
1. Login as administrator(user).
2. Change admin and user passwords, then login off and login to (superuser)
3. Check windows/mac update
* * *

### Software suite setup
1. Setup a keepass database (or Bitwarden). This is where you are going to save you unique passwords. THere is a password generated with the application included.
2. Setup yubikey for 2FA.
3. Setup a veracrypt container or thumbdrive to fit a backup of the software suite.
4. Setup and login into the chat applications recommended.
	- Wickr Enterprise
	- Cellcrypt
5. Setup and login VPNs
6. Setup VM on virtual box.
	- Setup Vdisk encryption in the VM setting screen.
	- Login to VM and change passwords for both root and user. Open a terminal
	`sudo su`
	`passwd` to change root
	`passwd user` to change user password
	Tip: (shift+ctrl+v to paste in terminal or right click to paste)
	- Check OS is updated.
	`sudo apt update && sudo apt dist-upgrade`
	- Install software to requirement.
	- Setup and test login VPN to SFTP server/
	- Setup SFTP server and test login.
7. Open FireFox and login into provided google accout. change password if it is a default password and go through security and sync settings. DO NOT setup 2FA. (This account is for admins to use as MDM) Admins document changed password.
8. Setup protonmail account. Use 2FA on this account.
7. Train how to use BleechBit and Eraser procedures.
* * *

## Traver router setup
1. Login into the admin page on brower. Check for updates.
2. Change wifi and admin page passwords.
3. Setup and test VPN
4. Limit DHCP to the total number of devices you plan on connecting
5. create a random MAC address. Try to mimic a actual device.
6. Override DNS of all clients.
7. Enable TLS over DNS to cloudflare.
* * *

## Phone and/or tablet setup
1. Login into the phone, check for updates and change PIN
2. Setup and login into the chat applications recommended.
	- Wickr Enterprise
	- Cellcrypt
	- Protonmail
3. Setup and login VPNs.
4. Train on how to copy over keepass database to phone. Use EDS to decrypt the thumbdrive if your using that.
5. Setup Secure folder feature in settings.
* * *

## Yubikey
Documentation - https://support.yubico.com/support/solutions
Downloads - https://www.yubico.com/products/services-software/download/
How-to - https://www.youtube.com/watch?v=Bh5Nf8sT6NE
 1. Download Yubikey Manager https://www.yubico.com/products/services-software/download/yubikey-manager/ 
 2. Insert the key configure a static key.
 3. Make use of Challenge-Reponse for local login or OTP
 4. Config OpenPGP keys
 5. Troubleshooting VM support. https://support.yubico.com/support/solutions/articles/15000008891-troubleshooting-device-passthrough-with-vmware-workstation-and-vmware-fusion


