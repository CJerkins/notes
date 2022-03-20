# Openvpn 

![41CRKpBzyBL.png](../../../_resources/41CRKpBzyBL.png)

## What is OpenVPN?
**REF:** *https://en.wikipedia.org/wiki/OpenVPN*

OpenVPN is open-source commercial[11] software that implements virtual private network (VPN) techniques to create secure point-to-point or site-to-site connections in routed or bridged configurations and remote access facilities. It uses a custom security protocol[12] that utilizes SSL/TLS for key exchange. It is capable of traversing network address translators (NATs) and firewalls. It was written by James Yonan and is published under the GNU General Public License Version 2 (GPLv2).[13]

OpenVPN allows peers to authenticate each other using pre-shared secret keys, certificates or username/password. When used in a multiclient-server configuration, it allows the server to release an authentication certificate for every client, using signatures and certificate authority. It uses the OpenSSL encryption library extensively, as well as the TLS protocol, and contains many security and control features. 


![openVpn_8066_8735.webp](../../../_resources/openVpn_8066_8735.webp)



## Conventional Terms
- "Site-to-site" and "Point-to-point" topology terms are interchangable. Refers to server to server connections.
- "Subnet" topology refer to a remote client to server connections.
- "Nested" topology refers to several site-to-site vpns concentrate to one centeral server.


![Rokonetworking.png](../../../_resources/Rokonetworking.png)


## Pros and Cons

Pro

1. Security levels are the highest – There are several security layers that protect you while you use OpenVPN, such as pre-shared keys, peer authentication and many other types of protection. Furthermore, due to the use of OpenSSL Library together with a thing called HMAC packet authentication, your network safety will be on the maximum and you will not have to worry about the bad people online. (This has its cons, this good security highly depends on proper configuration)
    
2. Reliable for its users – OpenVPN is said to be very reliable due to the fact that people who improve it and maintain it are the people who are responsible and who made sure that your data is always encrypted and never lost. This all can happen if OpenVPN disconnects for some reason – the whole network is then paused and all of your data is still safe.
    
3. The support of community all over the world – OpenVPN has a huge number of fans and a great worldwide community which can help everyone to use this type of VPN network. This is all made possible due to the fact that OpenVPN is an open-source piece of software that can be modified.


Con

1. There are certain proxy servers that are not compatible and do not support OpenVPN. 

2. Sometimes the latency can be high.User’s space plays important role in OpenVPN and basically all the encryption stages happen in this space. This problem may occur if you device is not strong enough to support all the processes going on, but if you have a strong machine, you need not worry about it.
    
3. Not really user friendly. OpenVPN has numerous options and it can be very confusing to someone who started using it. 

## Backends

**VPN providers and recommendations** *check out* https://thatoneprivacysite.net/
- Mullvad (Best, No KYC, anonymouse pay)
- ProtonmailVPN (Fair)
- PIA (Formerly recommended) *read* https://restoreprivacy.com/private-internet-access-kape-crossrider/

It really depends on your threat model. How do you want to look like?

**Selfhosting**

Hardware
- PFSense
- GliNet
- Openwrt
- RaspberryPi (dietpi)
This method has its limitations and requires a higher level of technical know how. Openvpn configuration files are feature loaded and compliacated to create.

![pfsenseOPEN.png](../../../_resources/pfsenseOPEN.png)


![gliopen.png](../../../_resources/gliopen.png)


Cloud hosted
- AWS - OpenVPN-AS instance
![openas.jpg](../../../_resources/openas.jpg)
	- EC2 micro - Ubuntu or Centos (1 core, 1 GB)
- Digital Ocean
- Google Cloud
- Azure
- Rackspace

This method is the versitile and cheapest. There are several scripts and instructions that can make deployment easier. Reference a few *here;*
- https://github.com/angristan/openvpn-install
- https://github.com/Nyr/openvpn-install
- https://github.com/StreisandEffect/streisand
- https://www.cyberciti.biz/faq/howto-setup-openvpn-server-on-ubuntu-linux-14-04-or-16-04-lts/

This instruction will demo an EC2 instance deployment and utilizing https://github.com/angristan/openvpn-install script. 


### End user (Clientside)

Openvpn is widely used and has clients software across all platforms. Highly recommend if you can do not us VPN server providers client apps. These app are embedded with adtech which defeats you purpose. There are app that can import the config file to make your connection to the server.
- Tunnelblick (MacOS - https://tunnelblick.net/cInstall.html)
- OpenVPN Connect (Windows - https://openvpn.net/client-connect-vpn-for-windows/)
- Gnome-OpenVPN - (Linux - https://www.ovpn.com/en/guides/ubuntu-gui)
- OpenVPN Android - (Android - playstore)
- OpenVPN Connect - (iOS - app store)
- OpenVPN Client - GliNet (Native)
- OpenVPN Client - PFSense (Native)

## Stunneling with openvpn
Stunnel is a proxy designed to add TLS encryption functionality to existing clients and servers without any changes in the programs' code. Its architecture is optimized for security, portability, and scalability (including load-balancing), making it suitable for large deployments. Disguises your vpn traffic to look like https.

Stunnel uses the OpenSSL library for cryptography, so it supports whatever cryptographic algorithms are compiled into the library. 

*Try this;* https://linuxscriptshub.com/install-stunnel4-work-openvpn-ubuntu/

## ShadowSocks with openvpn 
*read;* https://blog.fadyothman.com/bypassing-openvpn/

Disguises your vpn traffic to look like https.

*using mullvad;* https://mullvad.net/en/help/shadowsocks-openvpn-windows/

# Let's setup a server!






