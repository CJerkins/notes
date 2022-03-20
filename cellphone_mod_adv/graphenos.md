# GreaphenOS
REF https://grapheneos.org/

**Pros:**
****
- Degoogled
- No telemetry
- No Call back services
- Complete control of data and OS
- Increase battery life
- Verified Boot enabled
- Works with ATAK when encrypted

**Cons:**
****
- Not very user friendly for everyday user
- Lacks ability to chose google services
- Slowish
- Only works with Google Pixel Phones
- Lacks personization
- Needs high level IT knowledge to get certain phone features to work. (Commandline)

GrapheneOS is an open source privacy and security focused mobile OS with Android app compatibility. It's focused on the research and development of privacy and security technology including substantial improvements to sandboxing, exploit mitigations and the permission model. GrapheneOS also develops various apps and services with a focus on privacy and security. Vanadium is a hardened variant of the Chromium browser and WebView specifically built for GrapheneOS. GrapheneOS also includes our minimal security-focused PDF Viewer, our hardware-based Auditor app / attestation service providing local and remote verification of devices, and the externally developed Seedvault encrypted backup which was initially developed for inclusion in GrapheneOS.

GrapheneOS is a collaborative open source project, not a company. It's used and supported by a variety of companies and other organizations. It won't be closely tied to any company in particular. There will eventually be a non-profit GrapheneOS foundation, but for now the developers represent the project.

GrapheneOS improves the privacy and security of the OS from the bottom up. It has a hardened kernel, libc, malloc and compiler toolchain with many low-level improvements. These changes are designed to eliminate whole classes of serious vulnerabilities or provide meaningful barriers to exploitation. We avoid making changes without a clear rationale and we regularly work towards simplifying and replacing these low-level improvements. The malloc implementation is our own hardened_malloc providing cutting edge security for modern systems. The hardened_malloc project is portable to other Linux-based operating systems and is being adopted by other security-focused operating systems like Whonix. The hardened_malloc README has extensive documentation on it. Our work also heavily influenced the design of the next-generation musl malloc implementation which offers substantially better security than musl's previous malloc while still having minimal memory usage and code size.

There are also many under-the-hood changes at a higher level, including major improvements to SELinux policies particularly for the app sandbox. GrapheneOS tries to avoid impacting the user experience with the privacy and security features. Ideally, the features can be designed so that they're always enabled with no impact on the user experience and no additional complexity like configuration options. It's not always feasible, and GrapheneOS does add various toggles for features like the Network permission, Sensors permission, restrictions when the device is locked (USB peripherals, camera, quick tiles), etc. along with more complex user-facing privacy and security features with their own UX.

GrapheneOS has made substantial contributions to the privacy and security of the Android Open Source Project, along with contributions to the Linux kernel, LLVM, OpenBSD and other projects. Much of our past work is no longer part of the downstream GrapheneOS project because we've successfully landed many patches upstream. We've had even more success with making suggestions and participating in design discussions to steer things in the direction we want. Many upstream changes in AOSP such as removing app access to low-level process, network, timing and profiling information originated in the GrapheneOS project. The needs of the upstream projects are often different from ours, so they'll often reimplement the features in a more flexible way. We've almost always been able to move to using the upstream features and even when we still need our own implementation it helps to have the concepts/restrictions considered by the upstream project and apps needing to be compatible with it. Getting features upstream often leads to an improved user experience and app compatibility.

Official releases are available on the releases page and installation instructions are on the install page.

The official GrapheneOS releases are supported by our Auditor app and attestation service. The Auditor app and attestation service provide strong hardware-based verification of the authenticity and integrity of the firmware/software on the device. A strong pairing-based approach is used which also provides verification of the device's identity based on the hardware backed key generated for each pairing. Software-based checks are layered on top with trust securely chained from the hardware. For more details, see the about page and tutorial. These also support other operating systems.

## No Google apps or services

GrapheneOS will never include either Google Play services or another implementation of Google services like microG. Those are not part of the Android Open Source Project and are not required for baseline Android compatibility. Apps designed to run on Android rather than only Android with bundled Google apps and services already work on GrapheneOS, so a huge number of both open and closed source apps are already available for it.

Open APIs not tied to Google will continue to be implemented using open source providers like the Seedvault backup app. Text-to-speech, voice-to-text, non-GPS-based location services, geocoding, accessibility services, etc. are examples of other open Android APIs where we need to develop/bundle an implementation based on existing open source projects. Compatibility with apps depending on Google APIs / services will be improved by implementing them in a way that pretends Google has stopped existing and the servers are unavailable.


# Install
REF https://grapheneos.org/install

This is a guide on installing GrapheneOS for the officially supported devices. It can be followed for both the official releases and custom builds.

## Prerequisites

You should have at least 2GB of free memory available.

Windows 10, macOS Catalina, Arch Linux, Debian buster and Ubuntu 20.04 LTS are the officially supported operating systems for installing GrapheneOS. You should make sure your operating system is up-to-date before proceeding with these instructions. Older versions and other Linux distributions usually work, but if you encounter problems try using one of the officially supported options.

You need one of the officially supported devices. To make sure that the device can be unlocked to install GrapheneOS, avoid carrier variants of the devices. Carrier variants of Pixels use the same stock OS and firmware with a non-zero carrier id flashed onto the persist partition in the factory. The carrier id activates carrier-specific configuration in the stock OS including disabling carrier and bootloader unlocking. The carrier may be able to remotely disable this, but their support staff may not be aware and they probably won't do it. Get a carrier agnostic device to avoid the risk and potential hassle. If you CAN figure out a way to unlock a carrier device, it isn't a problem as GrapheneOS can just ignore the carrier id and it's otherwise the same.

It's best practice to update the stock OS on the device to make sure it's running the latest firmware before proceeding with these instructions. This avoids running into bugs, missing features or other differences in older firmware versions. Early Pixel 2 and Pixel 2 XL bootloader versions use a non-standard unlocking system not covered by these installation instructions. You can either update the device via over-the-air updates or sideload a full update, which for Pixel phones can be obtained from the full update package page.

These instructions use command-line tools. On Windows, use PowerShell rather than the legacy Command Prompt.
Obtaining fastboot

You need an updated copy of the fastboot tool and it needs to be included in your PATH environment variable. You can run fastboot --version to determine the current version. It must be at least 29.0.6. You can use a distribution package for this, but most of them mistakenly package development snapshots of fastboot, clobber the standard version scheme for platform-tools (adb, fastboot, etc.) with their own scheme and don't keep it up-to-date despite that being crucial.

List of distribution packages:

    - Arch Linux: android-tools provides fastboot and other useful tools not required for installation such as adb. android-udev provides udev rules allowing fastboot and adb to work in local sessions without root.
    - Debian: package is both broken and out-of-date, do not use (see paragraph above)
    - Ubuntu: package is both broken and out-of-date, do not use (see paragraph above)

### Standalone platform-tools

If your operating system doesn't make a proper version of fastboot available, consider using the [standalone releases of platform-tools from Google](https://developer.android.com/studio/releases/platform-tools). If you have the Android SDK or intend to do development work, you install the platform-tools package via the Android SDK package manager which can be used to keep it up-to-date. The Android SDK is available by itself or can be obtained via Android Studio.

To download, verify and extract the standalone platform-tools on Linux:
```
curl -O https://dl.google.com/android/repository/platform-tools_r30.0.4-linux.zip
echo '5be24ed897c7e061ba800bfa7b9ebb4b0f8958cc062f4b2202701e02f2725891  platform-tools_r30.0.4-linux.zip' | sha256sum -c
unzip platform-tools_r30.0.4-linux.zip
```
To download, verify and extract the standalone platform-tools on macOS:
```
curl -O https://dl.google.com/android/repository/fbad467867e935dce68a0296b00e6d1e76f15b15.platform-tools_r30.0.4-darwin.zip
echo 'SHA256 (fbad467867e935dce68a0296b00e6d1e76f15b15.platform-tools_r30.0.4-darwin.zip) = e0db2bdc784c41847f854d6608e91597ebc3cef66686f647125f5a046068a890' | shasum -c
tar xvf fbad467867e935dce68a0296b00e6d1e76f15b15.platform-tools_r30.0.4-darwin.zip
```
To download, verify and extract the standalone platform-tools on Windows:
```
curl.exe -O https://dl.google.com/android/repository/platform-tools_r30.0.4-windows.zip
(Get-FileHash platform-tools_r30.0.4-windows.zip).hash -eq "413182fff6c5957911e231b9e97e6be4fc6a539035e3dfb580b5c54bd5950fee"
tar xvf platform-tools_r30.0.4-windows.zip
```
Next, add the tools to your PATH in the current shell so they can be used without referencing them by file path, enabling usage by the flashing script.

On Linux and macOS:
```
export PATH="$PWD/platform-tools:$PATH"
```
On Windows:
```
$env:Path = "$pwd\platform-tools;$env:Path"
```
Sample output from fastboot --version afterwards:
```
fastboot version 30.0.4-6686687
Installed as /home/username/downloads/platform-tools/fastboot
```
This is a temporary change to PATH for the current shell and will need to be done again if you open a new terminal. Make sure that the fastboot command works in the current shell before trying to run the flashing script.

### Obtaining signify

To verify the download of the OS beyond the security offered by HTTPS, you can use the signify tool. If you do not have a way to obtain signify from a package repository you're already trusting, it does not make sense to use it. GrapheneOS releases are hosted on our servers and we do not have third party mirrors. A compromised signify would be able to compromise your OS and the GrapheneOS download due to the lack of an application security model on traditional operating systems. It would be worse than not trying to verify the signatures. It's far less likely that our servers would be compromised than someone's GitHub account or GitHub itself. You're already trusting these installation instructions from our site, which is hosted on the same static web server infrastructure as the releases.

List of distribution packages:

    - Arch Linux: signify
    - Debian: signify-openbsd with the command renamed to signify-openbsd
    - Ubuntu: signify-openbsd with the command renamed to signify-openbsd

On Debian-based distributions, the signify package and command are an unmaintained mail-related tool for generating mail signatures (not cryptographic signatures) with the final releases from 2003-2004 made directly by the developer via the Debian package without upstream releases. Please pressure them to correct this usability issue.

## Enabling OEM unlocking

OEM unlocking needs to be enabled from within the operating system.

Enable the developer options menu by going to Settings ➔ About phone and pressing on the build number menu entry until developer mode is enabled.

Next, go to Settings ➔ System ➔ Advanced ➔ Developer options and toggle on the 'Enable OEM unlocking' setting. This requires internet access on devices with Google Play services as part of Factory Reset Protection (FRP) for anti-theft protection.
Unlocking the bootloader

First, boot into the bootloader interface. You can do this by turning off the device and then turning it on by holding both the Volume Down and Power buttons.

Unlock the bootloader to allow flashing the OS and firmware:
```
fastboot flashing unlock
```
The command needs to be confirmed on the device and will wipe all data.

## Obtaining factory images

You need to obtain the GrapheneOS factory images for your device to proceed with the installation process.

You can either download the files with your browser or using a command like curl. It's generally easier to use the command-line since you're already using it for the rest of the installation process, so these instructions use curl. On Windows, you need to reference curl as curl.exe since PowerShell has a legacy curl alias.

Download the factory images public key (factory.pub) in order to verify the factory images:
```
curl -O https://releases.grapheneos.org/factory.pub
```
This is the content of factory.pub:

untrusted comment: GrapheneOS factory images public key
`RWQZW9NItOuQYJ86EooQBxScfclrWiieJtAO9GpnfEjKbCO/3FriLGX3`

The public key has also been published via the official @GrapheneOS Twitter account, the /u/GrapheneOS Reddit account and is available on GitHub. When the current signing key is replaced, the new key will be signed with it.

Download the factory images for the device from the releases page. For example, to download the 2020.05.05.02 release for the Pixel 3 XL (crosshatch):
```
curl -O https://releases.grapheneos.org/crosshatch-factory-2020.05.05.02.zip
curl -O https://releases.grapheneos.org/crosshatch-factory-2020.05.05.02.zip.sig
```
Verify the factory images using the signature if you were able to obtain signify from trusted package repositories (see above):
```
signify -Cqp factory.pub -x crosshatch-factory-2020.05.05.02.zip.sig && echo verified
```
This will output verified if verification is successful. If something goes wrong, it will output an error message rather than verified.
## Flashing factory images

The initial install will be performed by flashing the factory images. This will replace the existing OS installation and wipe all the existing data.

Reboot into the bootloader interface to begin the flashing procedure.

Next, extract the factory images.

On Linux:
```
unzip crosshatch-factory-2020.05.05.02.zip
```
On macOS and Windows:
```
tar xvf crosshatch-factory-2020.05.05.02.zip
```
Move into the directory:
```
cd crosshatch-factory-2020.05.05.02
```
Flash the images with the flash-all script in the directory.

On Linux and macOS:
```
./flash-all.sh
```
On Windows:
```
./flash-all.bat
```
Wait for the flashing process to complete and proceed to locking the bootloader before using the device as locking wipes the data again.

## Troubleshooting

A common issue on Linux distributions is that they mount the default temporary file directory /tmp as tmpfs which results in it being backed by memory and swap rather than persistent storage. By default, the size is 50% of the available virtual memory. This is often not enough for the flashing process, especially since /tmp is shared between applications and users. To use a different temporary directory if your /tmp doesn't have enough space available:
```
mkdir tmp
TMPDIR="$PWD/tmp" ./flash-all.sh
```
A majority of failed flashes tend to be caused by substandard USB connectors, plugging in via hubs or bad cables which aren't properly up to the USB standard. The scrollback from a failed flash will contain valuable diagnostic information which is essential in knowing where and how the process went wrong.

Front I/O ports on desktop computer cases and USB 3.1 or USB C on many laptops often aren't implemented properly or are broken in subtle ways, which may cause flashing to fail even on a USB port that works for other peripherals. Older Linux kernels that predate version 5 may have inadequate or patchwork support for USB C or USB 3. If you are installing from a Linux distribution, ensure your distribution uses a modern kernel.

Always use a high quality USB A to USB C cable with a rear USB port directly on your motherboard, and never use a USB hub for flashing. Never install from a virtual machine; USB passthrough in software emulation may be broken or inadequate and this can cause the flashing to fail.

## Locking the bootloader

Locking the bootloader is important as it enables full verified boot. It also prevents using fastboot to flash, format or erase partitions. Verified boot will detect modifications to any of the OS partitions (vbmeta, boot/dtbo, product, system, vendor) and it will prevent reading any modified / corrupted data. If changes are detected, error correction data is used to attempt to obtain the original data at which point it's verified again which makes verified boot robust to non-malicious corruption.

In the bootloader interface, set it to locked:
```
fastboot flashing lock
```
The command needs to be confirmed on the device since it needs to perform a factory reset.

Unlocking the bootloader again will perform a factory reset.

## Disabling OEM unlocking

OEM unlocking can be disabled again in the developer settings menu within the operating system after booting it up again.

## Verifying installation

Verified boot authenticates and validates the firmware images and OS from the hardware root of trust. Since GrapheneOS supports full verified boot, the OS images are entirely verified. However, it's possible that the computer you used to flash the OS was compromised, leading to flashing a malicious verified boot public key and images. To detect this kind of attack, you can use the Auditor app included in GrapheneOS in the Auditee mode and verify it with another Android device in the Auditor mode. The Auditor app works best once it's already paired with a device and has pinned a persistent hardware-backed key and the attestation certificate chain. However, it can still provide a bit of security for the initial verification via the attestation root. Ideally, you should also do this before connecting the device to the network, so an attacker can't proxy to another device (which stops being possible after the initial verification). Further protection against proxying the initial pairing will be provided in the future via optional support for ID attestation to include the serial number in the hardware verified information to allow checking against the one on the box / displayed in the bootloader. See the Auditor tutorial for a guide.

After the initial verification, which results in pairing, performing verification against between the same Auditor and Auditee (as long as the app data hasn't been cleared) will provide strong validation of the identity and integrity of the device. That makes it best to get the pairing done right after installation. You can also consider setting up the optional remote attestation service.

## Replacing GrapheneOS with the stock OS

Installation of the stock OS via the stock factory images is the same process described above. However, before locking, there's an additional step to fully revert the device to a clean factory state.

The GrapheneOS factory images flash a non-stock Android Verified Boot key which needs to be erased to fully revert back to a stock device state. After flashing the stock factory images and before locking the bootloader, you should erase the custom Android Verified Boot key to untrust it:
```
fastboot erase avb_custom_key
```