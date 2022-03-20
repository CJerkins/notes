OS - debian
network - dmz
IP - 10.112.255.75
username - debian
sshkey - local.vanguard.net
Software installed
- openvpn
- vim
- wireguard
- pihole


**IPTables**

```
sudo iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -p icmp -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 22 -m conntrack --ctstate NEW -m comment --comment "Accept SSH" -j ACCEPT
sudo iptables -A INPUT -p udp -m udp --sport 68 --dport 67 -m comment --comment "Accept dhcp if needed" -j ACCEPT
sudo iptables -A INPUT -i con0 -p udp -m udp --dport 11194 -m comment --comment "Accept fight VPN from con0" -j ACCEPT
sudo iptables -A INPUT -i con0 -p udp -m udp --dport 11195 -m comment --comment "Accept lighter VPN from con0" -j ACCEPT
sudo iptables -A INPUT -j LOG --log-prefix "iptables input dropped: "
```
```
sudo iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -m conntrack --ctstate INVALID -j DROP
sudo iptables -A FORWARD -p icmp -j ACCEPT
sudo iptables -A FORWARD -i tm+ -o tm+ -j ACCEPT
sudo iptables -A FORWARD -i tm+ -o eg0 -j ACCEPT
sudo iptables -A FORWARD -i eth1 ! -o eth0 -m comment --comment "ens34 traffic goes anywhere except via ens33" -j ACCEPT
```
## Forwarding to backend
```
sudo iptables -I FORWARD 6 -i tm+ -o ens34 -d 172.1611.10/32 -j ACCEPT
```
```
sudo iptables -A OUTPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -m conntrack --ctstate INVALID -j DROP
sudo iptables -A OUTPUT -o lo -j ACCEPT
sudo iptables -A OUTPUT -p icmp -j ACCEPT
sudo iptables -A OUTPUT -p udp --sport 67 --dport 68 -m comment --comment "Allow DHCP if needed" -j ACCEPT
sudo iptables -A OUTPUT -o eg0 --ctstate NEW -m comment --comment "Accept dns and updates" -j ACCEPT
sudo iptables -A OUTPUT -o con0 -p tcp -m tcp --dport 20022 -m conntrack --ctstate NEW -m comment --comment "Accept ssh to con0" -j ACCEPT
sudo iptables -A OUTPUT -d 138.197.168.186/32 -o ens33 -p udp -m udp --dport 30000 -m comment --comment "Accept tree vpn to go out to con0" -j ACCEPT
sudo iptables -A OUTPUT -d 10.1.3.2/32 -o con0 -p udp -m udp --dport 30004 -m comment --comment "allows spoon vpn to built through con0" -j ACCEPT
sudo iptables -A OUTPUT -j LOG --log-prefix "iptables output dropped: "
```
```
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT DROP
```
```
sudo iptables -t nat -A POSTROUTING -o eg0 -m comment --comment "allows traffic to appear coming from fe3" -j SNAT --to-source 10.1.4.2
```


## VPN Servers

## VPN clients
**UNIX tunnel** - p2p - main pipe
IP - 10.50.0.2


**GNU tunnel** - p2p - egress tunnel