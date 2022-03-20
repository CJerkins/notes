****Last update 03AUG2020****

This tutorial is going to be a step by step how to install required software and hardening for a Safe Travelers Kit. Note the last update to tut from relavance.

There is an assumption that you have a basic working knowledge on how to install an operating system and navigate system settings. This tutorial will not go in detail on how to do it but at least leave references to help with those details. There is alot of material out there, there is no need to regergetate it all again. 

### Requirements
Internet connectivity (There isn't much need for this other than help research references and updates)

**Laptop min specs**
i3 or greater
8GB of RAM
160 GB ssd
Ethernet and wifi adapters
16GB USB thumb drive
(The following will be in safe traveler install folder)
latest windows 10 iso
latest software package

**Mobile Phone**
Any Andriod OS based phone will work with our recommendations. iPhones will work to but not all the recommendations. Best recommendations from this office is either a Google Pixel or Iphone. Samsungs have alot of third party bloatware you are never sure were it is reaching out to. 

**Travel Router**
Any small router capible to flash OpenWRT. Don't get wrap if it is from china or not.

# Windows OS Install and Hardening

**OS installation**
Here we are going to start with the Windows OS intall and hardening from scratch. Normally we will have the image on a PXE and create the kits from that.

On a seperate working Windows10 machine plug-in your harden drive with the installation software locate windows .iso in safe_traveler/usb_installer. Double click the Rufus program to execute the program. SEE REF https://www.windowscentral.com/how-create-windows-10-usb-bootable-media-uefi-support
https://www.youtube.com/watch?v=d1eBv4hFNW8

SEE REF How to install windows from USB **NOTE make sure you chose the Windows10 Pro option https://www.youtube.com/watch?v=l9GnWYKyyYo

After install is complete make sure you decline all the options that want you to give up data to microsoft. DO NOT create a microsoft accout, you don't need it.

**Windows Hardening**
*Can use cheater scrpits like Pravatezilla https://github.com/builtbybel/privatezilla*
Before connecting to the internet, downloading programs, updating, and basicly dirtying up the system lets implement some hardening methods off the bat. All of which is done on the admin user.

Ensure there is 2 users. 1 is admin the other local.

**Disable Cortana**
Follow this tut for Window 10 Pro. SEE REF https://www.lifewire.com/turn-off-cortana-in-windows-10-4165920

`HKEY_Local_Machine/SOFTWARE/Policies/Microsoft/Windows`

**Disable all privacy settings**
In Settings > Privacy (all the way to the bottem). Starting from the top go through and disable everything. Go through the app permissions and disable any permission you think you won't need. On default kit provided by 6x all are disabled.

**Disable The Page File**
https://www.craigthetechteacher.com/how-to-properly-set-or-disable-the-windows-10-paging-file-2020/
Make sure to chose no paging files.

**Disable Fast Startup and Hybrid Sleep**
https://www.windowscentral.com/how-disable-windows-10-fast-startup
https://www.pugetsystems.com/labs/support-software/How-to-disable-Sleep-Mode-or-Hibernation-793/

**Disable remote access**
https://www.securicy.com/blog/how-to-disable-remote-access-in-windows-10/
Then in search bar type `advance system settings` choose top option. Click the remote tab and uncheck Allow Remote Assistance box.

**Check firewall is enable** 
In search bar type "Firewall" click top option. Check through the menu that everything is on and Windows Defender updated. Check on which apps are allowed through the firewall. Some will be depending which are in use. IE: Filezilla.

Setup a non-admin user account. This account is going to be going to be the account mostly used. Both accounts require good passwords.
https://support.microsoft.com/en-us/help/4026923/windows-10-create-a-local-user-or-administrator-account
Once you create the new user you have to login in and repeat the privacy setting feature again

**Turn on Bitlocker**
https://support.microsoft.com/en-us/help/4028713/windows-10-turn-on-device-encryption
Setup bitlocker at issue

**Recommended software installation**
The recommended software list is only a recommendation. Software changes rapidly. Some are good for a while then it is not. Most of the software in the list have portable versions which means you can save the programs on a USB or on Desktop then create a shortcut on Desktop to execute them. This keeps the program being installed on window filesystem. This makes it easier to maintian versions and easy deleted completely if needed. Below is the current list. check out https://portableapps.com/
	- Veracrpt(Portable)
	- Cellcrypt
	- Wickr enterprise
	- Joplin(Portable)
	- Bleechbit(Portable)
	- Eraser
	- Keepass or Bitwarden(Portable)
	- OpenVPN
	- WireGuard
	- vlc
	- WinSCP (SFTP)
	- yubikey manager
	- yubikey authicator
	- VirtualBox(Portable)
	- Malwarebyte
	- Avira
	- TMAC

All software can be found on the software_suite folder

Setup bleachbit to clean everything. Setup Eraser to do 7passes

Uninstall as much bloatware as possible. If there is an app you do not plan on using delete. Apps like XBox Live on Windows and game streaming apps open firewall ports to work.
	
**Updating**
Ok now you can connect to the internet and update the computer. This may take a while and alot of data depending on how old your .iso was to start. In search bar type `check updates`, click first option. Click check for updates and get a BEER, or coffee. Also allow any of the software we installed to auto update as well.

**Browser setup and Hardening**
Recommended browser for this installation currently is Firefox. Either download install from internet or from the installation folder.

**Hardening**
https://restoreprivacy.com/firefox-privacy/
https://github.com/pyllyukko/user.js

**PlugIn/Extendtion Recommendations** 
Like for software as for recommendations goes for plugins. We only install a few, more does not equal better. You create a fingerprint the more you use at once. Below is our recommendation.
	- Ad Blocker - default
	- Https everywhere - default
	- Ghostery
	- Disconnect
	- noscript
	- Bitwarden

**VPN**
OpenVPN vs WireGuard. OpenVPN is older commonly use technology and is very compatible to several vpn services. Wireguard is a faster newer protocal quickly gaining popularity. You can obtain openvpn certs from SOCOM. POC is ?????

**TAK**
ATAK software will be included in the software suite but will not be installed. The safe travelers kit at defualt is built for Comms with teammates, family, and unclassified internet research.

**VM OS**
Any linux based OS will do. Recommend linux mint or centos8. Setup is similar to the windows 10 setup. 32GB virtual disk size.

Recommended software installation
	- Veracrpt
	`wget https://launchpad.net/veracrypt/trunk/1.24-update4/+download/veracrypt-1.24-Update4-setup.tar.bz2`
	`tar xjf veracrypt-1.24-Update4-setup.tar.bz2`
	`./veracrypt-1.24-Update4-setup-console-x64`
	- Wickr enterprise
	`sudo rm /etc/apt/preferences.d/nosnap.pref`
	`sudo apt-get update && sudo apt-get install snapd && sudo snap install wickrpro`
	- Joplin
	`sudo snap install joplin-james-carroll`
	- Keepass or Bitwarden
	`sudo apt-add-repository ppa:jtaylor/keepass`
	`sudo apt-get install keepass2 -y`
	- WireGuard
	`sudo apt install wireguard`
	- vlc
	`sudo snap install vlc`

**NOTE: if using snap packages you need to reboot afterwards.

PlugIn/Extendtion Recommendations. Firefox is normally install out of the box.
	- Ad Blocker - default
	- Https everywhere - default
	- Ghostery
	- Disconnect
	- noscript
	- Bitwarden

**Browser setup and Hardening**
Recommended browser for this installation currently is Firefox. Either download install from internet or from the installation folder.

**Hardening**
https://restoreprivacy.com/firefox-privacy/

Check firewall is on.

# Mac OS Install and Hardening
**OS installation**
https://support.apple.com/en-us/HT204904
Here there is no need for a usb boot on a MacBook the but we need to start from a fresh install. Start the mach holding Command (⌘)-R to go in recovery mode and install the lastest os.

**Mac OS Hardening**
Before connecting to the internet, downloading programs, updating, and basicly dirtying up the system lets implement some hardening methods off the bat. All of which is done on the admin user.

We will start at system preferences. Unlike windows we need an icloud account to use the apple app store to install application. These accounts will be unique to the computer not the user. 

Ensure there is 2 users. 1 is admin the other local. 

Under Security and Privacy, turn on filevault, and the firewall. Ensure in the advance settings the steath mode is checked.

**Recommended software installation**
The recommended software list is only a recommendation. Software changes rapidly. Some are good for a while then it is not. Most of the software in the list have portable versions which means you can save the programs on a USB or on Desktop then create a shortcut on Desktop to execute them. This keeps the program being installed on window filesystem. This makes it easier to maintian versions and easy deleted completely if needed. Below is the current list.
	- Veracrpt
	- Cellcrypt
	- Wickr enterprise
	- Joplin
	- Bleechbit
	- Eraser
	- Keepass or Bitwarden
	- OpenVPN
	- WireGuard
	- vlc
	- WinSCP(SFTP)
	- yubikey manager
	- Malwarebyte
	- Avira
	- TMAC

# Mobile Install

**Recommended software installation**
The recommended software list is only a recommendation. Software changes rapidly. Some are good for a while then it is not. Most of the software in the list are on the USB in mobile_software_suite. Below is the current list.

- OpenVPN or WireGuard
- EDS (Android veracypt variant)
- ProtonMail
- Secure Chat App (SOCOM Wickr Enterprise, Cellcrypt)
- Wigle, Gmon
- Keepass
- Authy
- Firefox
- Joplin
- Haven (Change detection)
- AndFTP

**Hardening Android** https://security.utexas.edu/handheld-hardening-checklists/android

**Browser setup and Hardening**
Recommended browser for this installation currently is Firefox. Either download install from internet or from the installation folder.

**Hardening**
https://restoreprivacy.com/firefox-privacy/

# Travel router
Current recommended travel router model is the Gl inet Slate with the out of the box os. Packet capture analysis indicates no phone home get/push http requests or odd protocols.

The router is good out of the box. 

**Hardening**

Recommended additional packages
- iptgeoip
- adblock

Configure firewall rules in advance section.
- Disable all the default rules except `Allow-IPSec-ESP` and `Allow-ISAKMP` under the firewall tab in traffic rules. 
- Comment out this section in custom rules
`#PPTP Passthrough`
`#iptables -t raw -D OUTPUT -p tcp -m tcp --dport 1723 -j CT --helper pptp`
`#iptables -t raw -A OUTPUT -p tcp -m tcp --dport 1723 -j CT --helper pptp`

deploy/streisand-existing-cloud-server.sh \
    --ip-address 3.131.237.79 \
    --ssh-user ubuntu \
    --site-config global_vars/noninteractive/amazon-site.yml

deploy/streisand-new-cloud-server.sh \
    --provider amazon \
    --site-config global_vars/noninteractive/amazon-site.yml
    












