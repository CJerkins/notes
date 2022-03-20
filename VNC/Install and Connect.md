https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-20-04

## Intall

`sudo apt update`
`sudo apt install xfce4 xfce4-goodies tightvncserver`
`vncserver`
`vncserver -kill :1`
`mv ~/.vnc/xstartup ~/.vnc/xstartup.bak`
`nano ~/.vnc/xstartup`
```
#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &
```
`chmod +x ~/.vnc/xstartup`
`vncserver -localhost`

## Connection

`ssh -L 59000:localhost:5901 -C -N -l sammy your_server_ip`

go to your client then connect localhost:59000

## Start on boot
`sudo nano /etc/systemd/system/vncserver@.service`

```
[Unit]
Description=Start TightVNC server at startup
After=syslog.target network.target

[Service]
Type=forking
User=sammy
Group=sammy
WorkingDirectory=/home/sammy

PIDFile=/home/sammy/.vnc/%H:%i.pid
ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/usr/bin/vncserver -depth 24 -geometry 1280x800 -localhost :%i
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```

`sudo systemctl daemon-reload`
`sudo systemctl enable vncserver@1.service`