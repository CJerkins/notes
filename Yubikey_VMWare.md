This guide covers the common issues with using a YubiKey or Security Key in a virtual machine (VM) with VMware Workstation and VMware Fusion.
Selecting the Correct Device

If you select the Shared device to passthrough, it will not work correctly. For example, when selecting the device to passthrough be sure to select the YubiKey device and not the Shared YubiKey device. If you are only shown Shared devices, you may need to update your virtual machine's .vmx configuration file as described below.
Updating the Virtual Machine's Configuration

    Shut down the virtual machine.
    Locate the VM's .vmx configuration file. For more information, see VMware's KB article on this.
    Open the configuration file with a text editor.
    Add the two lines below to the file and save it.

usb.generic.allowHID = "TRUE"
usb.generic.allowLastHID = "TRUE"

At this point, a non-shared YubiKey or Security Key should be available for passthrough. If not, you may need to manually specify the USB vendor ID and product ID in the configuration file as well. The example below applies to a YubiKey 4 or 5 with all its modes enabled. For more information, please see YubiKey USB ID Values.

usb.quirks.device0 = "0x1050:0x0407 allow"