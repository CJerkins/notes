install failtoban to prevent port scanning

Show tables
`sudo iptables -nvL`
`sudo iptables -t nat -nvL`
`sudo iptables -nvL --line-numbers`
delete a rule
`sudo iptables -D OUTPUT 7(line of the rule)`

change a rule location in the chain <-I>
`sudo iptables -I INPUT 1 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT`
replace rule <-R>

modify nat table
`sudo iptables -t nat -A POSTROUTING -s 10.101.0.0/24 -o eth0 comment --comment "Allow traffic from VPN subnet out eth0" -j MASQUERADE`

restore iptable rules to flush all rules
```
sudo iptable -p INPUT ACCEPT
sudo iptable -p OUTPUT ACCEPT
sudo iptable -F
```
if you modify the the rules file use it restore old rule after 30 secs, 10sec is default.
`sudo iptables-apply -t 30 rules.test`
to test and apply iptable rules.v4 file 
`sudo iptables-save` to view
`sudo iptables-save | sudo tee /etc/iptables/rules.v4` to save

```
sudo iptables-save > /tmp/rules.test
sudo iptables-apply -t 30 /tmp/rules.test
sudo iptables-save | sudo tee /etc/iptables/rules.v4
```

modify filter table

### Input Chain
```
sudo iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 20022 -m conntrack --ctstate NEW -m comment --comment "Accept SSH" -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 443 -m conntrack --ctstate NEW -m comment --comment "Accept OpenVPN" -j ACCEPT
sudo iptables -A INPUT -p icmp -j ACCEPT

```

### Output Chain
```
sudo iptables -A OUTPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -m conntrack --ctstate INVALID -j DROP
sudo iptables -A OUTPUT -o lo -j ACCEPT
```
***only needed if you want to box to connect to the internet***
```
sudo iptables -A OUTPUT -p tcp -m multiport --dports 80,443,53 -m conntrack --ctstate NEW -m comment --comment "Outbound HTTP, HTTPS, DNS" -j ACCEPT
sudo iptables -A OUTPUT -p udp -m multiport --dport 123,53 -m comment --comment "Outbound DNS" -j ACCEPT
sudo iptables -A OUTPUT -p icmp -j ACCEPT
```
### set policy
```
sudo iptables -P INPUT DROP
sudo iptables -P OUTPUT DROP
```

### Forward Chain
```
sudo iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -m conntrack --ctstate INVALID -j DROP
sudo iptables -A FORWARD -i mwr0 -o eth0 -s 10.101.0.0/24 -m comment --comment "Allow traffic from VPN subnet out eth0" -j ACCEPT
```
### set policy
```
sudo iptables -P FORWARD DROP
```


### NATing
```
sudo iptables -t nat -A POSTROUTING -o eth0 -m comment --comment "allow internet traffic back in" -j MASQUERADE
```

### IPv4 forwarding
```
sudo nano /etc/sysctl.conf
```
uncomment - net.ipv4.ip_forward=1
```
sudo sysctl -p
sudo sysctl --system
```

