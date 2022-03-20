Server config practial

`ip route show`
```
default via 192.168.11.2 dev ens33 proto static 
10.1.0.2 dev tun0 proto kernel scope link src 10.1.0.1 
192.168.11.0/24 dev ens33 proto kernel scope link src 192.168.11.40
```

ip route add breakdown
example:
`ip route add 10.0.0.0/24 via 10.1.0.1 dev eth0`
10.0.0.0/24=subnet or ipadd of the net or mach you want to connect
via = next hop which is a single ip addr typically the gw of that subnet.
dev=device routeing through on the machine

sudo iptables -t nat -A POSTROUTING -o eg0 -m comment --comment "allows traffic to appear coming from fe3" -j SNAT --to-source 10.50.2.2