REF https://docs.gl-inet.com/en/3/app/qdslrdashboard/

# About
qDslrDashboard is a Windows/Linux/MacOS and Android/iOS compatible app that lets you control your DSLR. Together with your GL-iNet router, you can also control your camera wirelessly.

# Client Downloads

Download the latest client version for your device from here: https://dslrdashboard.info/downloads/

# Installation Guide
At this point it is expected that your router works, and can connect to the internet without issues.

Install the client for your device from the download link above.
Login to your router and navigate to Applications -> Plug-ins
Click "update" in the top right:

Search for and install "ddserver"

To check if the installation was successful, go to the following url: http://192.168.8.1/cgi-bin/luci/admin/system/startup . If you have changed the default ip of your router, use that in the link above instead. Login and in the list, find ddserver and make sure it says "enabled" as bellow:

