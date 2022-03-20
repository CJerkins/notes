# WireGuard



![wireg.png](../../../_resources/wireg.png)



## What is WireGuard?
**REF:** *https://www.wireguard.com/*

WireGuard® is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN. WireGuard is designed as a general purpose VPN for running on embedded interfaces and super computers alike, fit for many different circumstances. Initially released for the Linux kernel, it is now cross-platform (Windows, macOS, BSD, iOS, Android) and widely deployable. It is currently under heavy development, but already it might be regarded as the most secure, easiest to use, and simplest VPN solution in the industry.

![wiredemo.jpeg](../../../_resources/wiredemo.jpeg)

## Audits
Conclusions - After several audits Linux Developers decide in March 2020 to include WireGuard in next kernal release 5.3.
**REF**
https://www.wireguard.com/formal-verification/
https://courses.csail.mit.edu/6.857/2018/project/He-Xu-Xu-WireGuard.pdf

## Conventional Terms
- "Site-to-site" and "Point-to-point" topology terms are interchangable. Refers to server to server connections.
- "Star" topology refer to a remote client to server connections.
- "Mesh" topology refers to several site-to-site vpns end to end with several server/client instances connected.

## Pros and Cons
**Pro**
-------
**Performance and speed**

1. WireGuard is supposed to provide more performance and bandwidth than the widely used IPsec and OpenVPN VPN protocols and software solutions. WireGuard uses the latest high-performance cryptography algorithms, such as the Noise Protocol Framework, Curve25519, ChaCha20, Poly1305, BLAKE2, SipHash24 or HKDF. And WireGuard gets even better performance from the fact that the software is executed as a Linux kernel module on the server side. 

**Easy configuration & cross-platform use**

2. One of the WireGuard goals is to make the software particularly easy to configure, such as SSH. WireGuard uses only public keys for identification and encryption and can therefore dispense with a certificate infrastructure. The VPN protocol can be used in a wide variety of applications, as there are cross-platform software solutions. Besides the Linux implementation there are ports for Android, macOS, FreeBSD, OpenBSD and Windows.

**Security**

3. WireGuard manages with very few lines of code. This is a great security advantage for two reasons: First, less code contains potentially fewer errors. On the other hand, less code is easier for security experts to verify. WireGuard currently contains about 4,000 lines of code.

**Cons**

**Work in Progress**

1. The developer has recently dubbed WireGuard no longer a work in progress but it is still unproven technology has yet reached an insitutional adoption. 2020 has been the first year VPN providers have widely implemented WG.

**Not usable without logs**

1. WireGuard has is no dynamic address management, the client addresses are fixed. That means we would have to register every active device and assign the static IP addresses on each VPN server. Last login timestamp for each device in order to reclaim unused IP addresses has to be logged as well.

## Backends

**VPN providers and recommendations** *check out* https://thatoneprivacysite.net/
- Mullvad (Best, No KYC, anonymouse pay)
- ProtonmailVPN (Fair)
- PIA (Formerly recommended) *read* https://restoreprivacy.com/private-internet-access-kape-crossrider/

It really depends on your threat model. How do you want to look like?

**Selfhosting**

Hardware
- PFSense (Package not yet in full release)
- GliNet
- Openwrt
- RaspberryPi (dietpi)
This method has its limitations and requires a higher level of technical know how. Openvpn configuration files are feature loaded and compliacated to create.

![WGC1.png](../../../_resources/WGC1.png)

Cloud hosted
- AWS - EC2 micro - Lightsail - Ubuntu or Centos (1 core, 1 GB)
- Digital Ocean
- Google Cloud
- Azure
- Rackspace

This method is the versitile and cheapest. There are several scripts and instructions that can make deployment easier. Reference a few *here;*
- https://github.com/angristan/wireguard-install
- https://github.com/psyhomb/wireguard-tools (Localtool)
- https://github.com/StreisandEffect/streisand
- https://www.wireguard.com/install/
- https://github.com/trailofbits/algo (Localtool)
- https://iliasa.eu/wireguard-how-to-access-a-peers-local-network/

This instruction will demo an EC2 instance deployment and utilizing https://github.com/angristan/wireguard-install script. 

### End user (Clientside)

Openvpn is widely used and has clients software across all platforms. Highly recommend if you can do not us VPN server providers client apps. These app are embedded with adtech which defeats you purpose. There are app that can import the config file to make your connection to the server.
- WireGuard (MacOS - https://itunes.apple.com/us/app/wireguard/id1451685025?ls=1&mt=12)
- WireGuard (Windows - https://download.wireguard.com/windows-client/wireguard-amd64-0.1.1.msi)
- WireGuard - (Linux - no gui)
- WireGuard Android - (Android - playstore)
- WireGuard Connect - (iOS - app store)
- WireGuard Client - GliNet (Native)
- WireGuard Client - PFSense (https://github.com/Ascrod/pfSense-pkg-wireguard)

Upcoming21& Oval2&Earplugs Cats
