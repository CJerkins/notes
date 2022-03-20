****Last update 20APR2021****
***
We are always looking for more interesting wifi survey tools. GliNet with openwrt is the perfect baseline to for such a tool. Tools with intergrated wifi/cellure boards on the device it self make this build a sinch. NOT!

Recommended device list:
1. E-750 Mudi
1. AR-750S Slate w/ USB GPS or serial interfaced GPS

Let's get started! With a fresh Glinet, set up wifi and plug in the internet to bring down packages. After the basic setup is complete lets get into the advanced setting GUI (Luci)

1. Go to the admin menu and activate ssh. Create some keys if you like then login via ssh on a terminal.
2. Update package list `opkg update`
3. Install packages `opkg install gpsd gpsd-clients kismet-server kismet-client netcat shadow-useradd kmod-usb-serial-pl2303 lcdproc`
4. Before starting kismet lets make sure the GPS is working and that GPSD is started. `gpsd -D 5 -N -n /dev/ttyUSB0`. It you get at least timestamps, your good, GPSD is working! Now lets start gpsd and enable it as a servicie.
	1. Edit `vi /etc/rc.local` press `i` to edit the file
	2. At the `gpsd /dev/ttyUSB0 -F /var/run/gpsd.sock`
	3. press `esc` then `:wq`
	4. Reboot the system and type `cgps` to make sure GPSD started on boot and is working.
5. Lets start and enable on boot kismet `service kismet_server enable && service kismet_server start`
6. Lets see kismet is working `kismet_client`

****Last update 20APR2021****
***
# Raspberry pi or Filet2 with ubuntu
https://pimylifeup.com/raspberry-pi-network-scanner/



***Filet2***
I stumbled on to another forum that said to try changine the default settings for gpsd at [/etc/default/gpsd] to this.

Default settings for the gpsd init script and the hotplug wrapper.

Start the gpsd daemon automatically at boot time
```
START_DAEMON="false"
#Use USB hotplugging to add new USB devices automatically to the #daemon

USBAUTO="true"
#Devices gpsd should collect to at boot time.
#They need to be read/writeable, either by user gpsd or the #group dialout.

DEVICES="/dev/ttyUSB0"
#Other options you want to pass to gpsd

GPSD_OPTIONS="-n -G -b" GPSD_SOCKET="/var/run/gpsd.sock"
#end of file gpsd
```
to my surprise it worked.

```
git clone https://github.com/aircrack-ng/rtl8812au.git
cd rtl8812au/
sudo make dkms_install
sudo modprobe 88XXau
```


# How to test the software (gpsd)
REF: https://gpsd.gitlab.io/gpsd/installation.html
You should start gpsd while running as root. Starting as a normal user will cause some loss of functionality. Starting with sudo will cause a different loss of functionality.

Start gpsd. You’ll need to give it as an argument a path to a serial or USB port with a GPS attached to it. Your test command should look something like this:

`gpsd -D 5 -N -n /dev/ttyUSB0`

Once gpsd is running, telnet to port 2947. You should see a greeting line that’s a JSON object describing GPSD’s version. Now plug in your GPS (or AIS receiver, or RTCM2 receiver).

Type '?WATCH={"enable":true,"json":true};' to start raw and watcher modes. You should see lines beginning with '{' that are JSON objects representing reports from your GPS; these are reports in GPSD protocol.

Start the xgps or cgps client. Calling it with no arguments should do the right thing. You should see a display panel with position/velocity-time information, and a satellite display. The displays won’t look very interesting until the GPS acquires satellite lock.

Have patience. If you are cold-starting a new GPS, it may take 15-20 minutes after it gets a good skyview for it to download an ephemeris for each satellites in view, and the current almanac. Only then can it deliver the best quality fixes.

A FAQ and troubleshooting instructions can be found at the GPSD project site.

# How to configure gpsd 
(raspberry pi or linux system)
I was having a similar problem. I did everything here and still couldn't get it to work in openCPN. I stumbled on to another forum that said to try changing the default settings for gpsd at /etc/default/gpsd to this.
```
# Default settings for the gpsd init script and the hotplug wrapper.

# Start the gpsd daemon automatically at boot time
START_DAEMON="false"

# Use USB hotplugging to add new USB devices automatically to the daemon
USBAUTO="true"

# Devices gpsd should collect to at boot time.
# They need to be read/writeable, either by user gpsd or the group dialout.
DEVICES="/dev/ttyUSB0"

# Other options you want to pass to gpsd
GPSD_OPTIONS="-n -G -b"
GPSD_SOCKET="/var/run/gpsd.sock"
#end of file gpsd
```
to my surprise it worked.
