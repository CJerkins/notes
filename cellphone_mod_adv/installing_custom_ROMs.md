# Tutorial How to flash a custom ROM on an Android phone

First thing, first we are going to collect our tools.

Requirements:
1. Windows Computer (Linux and Mac can be used but most of the tools are commonly supported in Windows)
2. Android device
3. Cable for your phone
4. SD card
5. Supporting Software
	1. Odin 3.0 or higher
	2. USB drivers (Samsung Phones normally need this driver installed)
	3. Custom [recovery](https://www.twrp.me)
	4. LineageOS ROM
6. List of not commonly needed software (This software is need sometimes depending on the device)
	1. ADB_Fastboot
	2. GAPPS (Google App installer)
	3. Magisk (root installer)
		1. KaliNetHunter
		2. Terminal

Copy and Paste the ROM or ROMs and optionally GAPPs .zip file and Magisk .zip files on a SD card. It is import these files stay in there compressed form. Insert SD Card in the phone.

## Prepare phone
This will only work with an unlocked phone you can access the setting menu. First we need to put the phone in developer mode. Some phones are different but most can be done by finding the Build ID number and taping it about 10times. There will be a notification when it works. With a Samsung phone this can be found in About Phone > System Information > Build ID. 

In developer mode back out and find the developer menu. Normally found under the about phone option. In this menu toggle "OEM unlock" and "USB debugger" to on. Now turn off the phone.

From "off", power on the to bootloader partition by holding "Power, Volume Down, Home button (The 3rd button maybe different depending on model)" down till it boots. Plug the usb cable in both computer and phone now.

## Install Odin3
Odin is used to upload Custom ROMs and bootloaders. Extract the contents and open the folder. Double click the .exe file to start the program. It will not need to be installed. At this momenet you can install the samsung drivers as well. Do this by extracting the .zip, open the folder and click the setup.exe to install.

Once completly installed in the log in Odin will say added and a blue bar on the top left corner will appear. If not check that bootloader has no errors, cable is pluged, drivers installed, and you have a serial connection. (Check this in device manager for a active 'com' port open)

## Install custom recovery
The most supported custom bootloader is [TWRP](https://www.twrp.me). we are going to use this to install the custom ROM. Download the appropraite image file for your device and save it to somewhere you can find it later. 

Open Odin click on the 'AP' button. Locate the TWRP image file and chose to upload. As long your phone is connected and comunicating with the computer press start to upload. It will only take 5 secs then will reboot. Depending on the model you will have to place the phone on recovery mode before it boots to the system image or the install will be corupted. You can do this typlically by holding down "Power, Volume UP, and home buttons"

## Install custom ROM (Operating system)
With phone booted to TWRP recovery. With this menu you will see an install option. In there you chose the from the SD card, the .zip file of the ROM you downloaded earlier. After this you can chose the additional .zip files (Magisk or GAPPs) to install. You can install these after flashing new ROM, it doesn;t need to happen during the flashing process. 

You may have issue with /preload partition not mounting errors. This is from there not being a 'preload' partition most likely. Follow this tut for assistance. Other errors may occur and this maybe just a error in the intial flashing of TWRP image. Re do the last step this may solve your problem. 

After flash is complete, reboot like normal to your new custom ROM

**CHEERS!**
