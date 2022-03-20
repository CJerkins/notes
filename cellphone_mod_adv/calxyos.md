# Calxyos

REF https://calyxos.org/what https://calyxos.org/who

**Pros:**
****
- Degoogled
- No telemetry
- No Call back services
- Complete control of data and OS
- Increase battery life
- Verified Boot enabled
- Fast
- Option to active google services
- Options for preinstall apps
- Works with ATAK when encrypted

**Cons:**
****
- Not very user friendly for everyday user
- Only works with Google Pixel Phones
- Lacks personalization settings
- Some advertized features don't work

Everyone needs a phone.  Not everyone wants governments and corporations tracking their every move and spying on their communications.  CalyxOS lets you have your cake and eat it too, with  "Privacy by Design"

In social science, agency is defined as: the capacity of individuals to act independently and to make their own free choices.  There is a whole community of free software developers working to build tools that enable us to take back our privacy, and we have curated a collection of the "best of the best" in order to give you back your agency.

When you use a phone with CalyxOS:

    - you are empowered by the combined expertise of the Internet privacy community
    - 'de-googled' Android. does not put your data in google's cloud or constantly report your location to google
    - phone calls are opportunistically encrypted so nobody can listen in
    - you are warned when you make or receive an unencrypted phone call
    - text messages are encrypted and a timer can be set so they disappear
    - advertising and trackers blocked via DuckDuckGo Browser and DuckDuckGo as default search provider
    - web browsing is anonymized via Tor Browser
    - built-in free "Virtual Private Network" services from trusted organizations protect you from being spied on
    - your phone is receiving regular, timely, automatic security updates
    - your data is backed up with strong encryption to your personal cloud server or to USB storage

CalyxOS minimizes the tracking, surveillance, and spying done by phone manufacturers, mobile phone service providers, internet service providers, advertising companies, data miners, and malicious hackers. The operating system is designed to ensure maximum usability and flexibility, so that you have an array of choices available to ensure your security and privacy.

CalyxOS is designed with the needs of human rights frontline defenders, journalists, lawyers, and political and social activist groups in mind.

REF https://calyxos.org/installation
# Installation Instructions
Submitted by cdesai on Mon, 08/26/2019 - 17:11

## Install

### Prerequisites

You should have at least 2GB of free memory available on the PC.
You need the unlocked variant of one of the supported devices, not a locked carrier specific variant.
It's best practice to update the stock OS on the device to make sure it's running the latest version before proceeding with these instructions. You can just connect the device to the internet, and let it auto update to the latest version available. You can check for updates by opening the Settings app, then near the bottom tapping System ➔ Advanced ➔ System Update.

### Obtaining fastboot

You need an updated copy of the [platform-tools](https://developer.android.com/studio/releases/platform-tools) and it needs to be included in your **"PROGRAM PATH"** environment variable.

You can download the official releases of platform-tools from Google. You can either obtain these as part of the standalone SDK or Android Studio. For one time usage, it's easiest to obtain the latest standalone platform-tools release, extract it and add it to your **"PROGRAM PATH"** in the current shell. For example:

```
unzip platform-tools_r30.0.4-linux.zip
export PATH="PATH TO/platform-tools:$PATH"
```
Example: 
`export PATH="/Users/username/platform-tools:$PATH"`

Sample output from
`fastboot --version`
afterwards:
```
fastboot version 30.0.4-6686687
Installed as /home/username/platform-tools/fastboot
```
Don't proceed with the installation process until this is set up properly in your current shell. Also, make sure the version matches the above or is newer/higher than this, as older versions are unsuitable for flashing.

### Enabling OEM unlocking

OEM unlocking needs to be enabled from within the operating system.
Enable the developer settings menu by going to Settings ➔ System ➔ About phone and tapping on the build number menu entry until developer mode is enabled.Next, go to Settings ➔ System ➔ Advanced ➔ Developer settings and toggle on the 'Enable OEM unlocking' setting. This may require internet access on some devices

### Unlocking the bootloader

First, reboot into the bootloader interface. You can do this by turning off the device and then turning it on by holding both the Volume Down and Power buttons.
The bootloader now needs to be unlocked to allow flashing new images:
```
fastboot flashing unlock
```
The command needs to be confirmed on the device, using the volume and power buttons.

### Flashing factory images

Reboot into the bootloader interface to begin the flashing procedure.
Next, extract the factory images and run the script to flash them. Note that the `fastboot` command run by the flashing script requires a fair bit of free space in a temporary directory, which defaults to
/tmp
:
```
unzip crosshatch-factory-2020.08.04.12.zip
cd crosshatch-qq3a.200805.001
./flash-all.sh
```
Use a different temporary directory if your
/tmp
doesn't have enough space available:
```
mkdir tmp
TMPDIR="$PWD/tmp" ./flash-all.sh
```
Example: 
`TMPDIR="/Users/username/crosshatch-qq3a.200805.001/tmp" ./flash-all.sh`

Wait for the flashing process to complete and for the device to boot up using the new operating system.
You should now proceed to locking the bootloader before using the device as locking wipes the data again.

### Re-locking the bootloader

Re-locking the bootloader is important as it enables full verified boot. It also prevents using fastboot to flash, format or erase partitions. Verified boot will detect modifications to any of the OS partitions and it will prevent reading any modified / corrupted data. In the bootloader interface, set it to locked:

`fastboot flashing lock`

The command needs to be confirmed on the device since it needs to perform a factory reset.
Unlocking the bootloader again will perform a factory reset.

### Disabling OEM unlocking

OEM unlocking can be disabled again in the developer settings menu within the operating system after booting it up again.

At this point, your device should be all setup and ready for usage with CalyxOS.

### Replacing CalyxOS with the stock OS

Installation of the stock OS via the stock factory images is the same process described above. However, before locking, there's an additional step to fully revert the device to a clean factory state on modern devices like the Pixel 2, Pixel 2 XL, Pixel 3, Pixel 3 XL, Pixel 3a and Pixel 3a XL.
This step needs to be performed by rebooting to the bootloader like before, and then running:
```
fastboot erase avb_custom_key
```
