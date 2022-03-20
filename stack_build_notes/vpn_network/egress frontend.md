Instance - egress.vanguard.net
vpc.id - vpc-0a15c0518e3a7435b
POS - Frankfurt

OS - ubuntu
network - Public
Public IP - 18.196.243.115
Internal IP - 10.1.1.86
username - ubuntu
sshkey - aws.de
ssh port - 2020
Software installed
- openvpn
- vim
- wireguard


**IPTables**
```
sudo iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -p icmp -j ACCEPT
sudo iptables -A INPUT -p udp --sport 68 --dport 67 -m comment --comment "Allow DHCP if needed" -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 2020 -m conntrack --ctstate NEW -m comment --comment "accept ssh" -j ACCEPT
sudo iptables -A INPUT -i con0 -p udp -m udp --dport 9432 -m comment --comment "allow egress vpn in through con0" -j ACCEPT
sudo iptables -A INPUT -j LOG --log-prefix "iptables input dropped: "
```
```
sudo iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -m conntrack --ctstate INVALID -j DROP
sudo iptables -A FORWARD -i gw0 -o eth0 -m comment --comment "forward internet traffic from gw0 to eth0" -j ACCEPT
sudo iptables -A FORWARD -p icmp -j ACCEPT
sudo iptables -A FORWARD -j LOG --log-prefix "iptables forward dropped: "
```
```
sudo iptables -A OUTPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -m conntrack --ctstate INVALID -j DROP
sudo iptables -A OUTPUT -o lo -j ACCEPT
sudo iptables -A OUTPUT -p icmp -j ACCEPT
sudo iptables -A OUTPUT -p udp --sport 67 --dport 68 -m comment --comment "Allow DHCP if needed" -j ACCEPT
sudo iptables -A OUTPUT -p tcp -m multiport --dports 53,80,443 -m conntrack --ctstate NEW -m comment --comment "accept dns and updates" -j ACCEPT
sudo iptables -A OUTPUT -p udp -m multiport --dports 53,123 -m comment --comment "accept dns and ntp" -j ACCEPT
sudo iptables -A OUTPUT -d 3.133.144.175/32 -p udp -m udp --dport 4241 -m comment --comment "Accept BSD vpn out to con0" -j ACCEPT
sudo iptables -A OUTPUT -j LOG --log-prefix "iptables output dropped: "
```
```
sudo iptables -t nat -A POSTROUTING -o eth0 -m comment --comment "allow internet traffic back in" -j MASQUERADE
```
```
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT DROP
```
## VPN Servers
-------------------
**GNU tunnel** - p2p - from gw to egress
VPN port - 9432
Tunnel IP - 10.50.2.0/32

## VPN Clients
-------------------
**BSD tunnel** - p2p - to con