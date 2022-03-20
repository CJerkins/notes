`sudo apt update -y && sudo apt upgrade -y`

`sudo apt dist-upgrade`

run a command in the background
`sudo openvpn --config /etc/openvpn/bear-server.conf &`
`bg` = run last command in background
`jobs` = list running jobs(commands)
`fg` = bring background command to forground

`sudo tcpdump -ni ens33 udp`

secure copy from a remote to a remote
`scp -3 ca:/tmp/bear-server.tar ps:/tmp/`
### reuse command quickys
changes a keyword in the last command then runs the command
`^start^stop`
example:
```
student1@practice-client:~$ sudo systemctl enable openvpn@bear-client
student1@practice-client:~$ ^enable^disable
sudo systemctl disable openvpn@bear-client
Removed /etc/systemd/system/multi-user.target.wants/openvpn@bear-client.service.
```
google - reverse i search
ctl r then type keyword then ctl r till you find your command

`history`
then bang and line number
`!69`

### VIM quickys
forget to sudo with vim
`:w ! sudo tee %`

jump to line example
`5gg`

### Burining the VPS
`sudo systemctl stop openvpn@jacket-server`
shred server conf
`sudo shred -vvzun 7 /etc/openvpn/jacket*`
`sudo shred -vvzun 7 /etc/openvpn/*`

shred logs and inspect
`cd /var/log`

`sudo grep -i 'accept' auth.log`

`sudo find /var/log -type f -exec sudo shred -vvzun 5 "{}" +`
write zeros to the disk
`nohup sudo dd if=/dev/zero of=/dev/vda & disown`

### to leave no history
leave a space in front of command

```
sudo mkdir -p /etc/openvpn/unix-jail/tmp
sudo chmod 1777 /etc/openvpn/un-jail/tmp
```