REF https://github.com/thinkst/opencanary

For updated and cleaner documentation, please head over to http://opencanary.org

Installation on Ubuntu:
```
sudo apt-get install python-dev python-pip python-virtualenv
virtualenv env/
. env/bin/activate
pip install opencanary
```

To start out you need to generate a config file
```
virtualenv env
. env/bin/activate
opencanaryd --copyconfig
nano ~/.opencanary.conf
```

Configure opencanary.conf
```
{
    "device.node_id": "hostname.domain.net",
    "git.enabled": false,
    "git.port" : 9418,
    "ftp.enabled": true,
    "ftp.port": 21,
    "ftp.banner": "FTP server ready",
    "http.banner": "Apache/2.2.22 (Ubuntu)",
    "http.enabled": true,
    "http.port": 80,
    "http.skin": "nasLogin",
    "httpproxy.enabled" : false,
    "httpproxy.port": 8080,
    "httpproxy.skin": "squid",
    "logger": {
        "class": "PyLogger",
        "kwargs": {
            "formatters": {
                "plain": {
                    "format": "%(message)s"
                }
            },
            "handlers": {
                "SMTP": {
                    "class": "logging.handlers.SMTPHandler",
                    "mailhost": ["smtp.gmail.com", 587],
                    "fromaddr": "noreply@vanguard.net",
                    "toaddrs" : ["vanguardnet6@gmail.com"],
                    "subject" : "OpenCanary Alert",
                    "credentials" : ["email@gmail.com", "supersecretpassword"],
                    "secure" : []
                },
                "console": {
                    "class": "logging.StreamHandler",
                    "stream": "ext://sys.stdout"
                },
                "file": {
                    "class": "logging.FileHandler",
                    "filename": "/var/tmp/opencanary.log"
                }
            }
        }
    },
    "portscan.enabled": true,
    "portscan.logfile":"/var/log/kern.log",
    "portscan.synrate": 5,
    "portscan.nmaposrate": 5,
    "portscan.lorate": 3,
    "smb.auditfile": "/var/log/samba-audit.log",
    "smb.enabled": false,
    "mysql.enabled": false,
    "mysql.port": 3306,
    "mysql.banner": "5.5.43-0ubuntu0.14.04.1",
    "ssh.enabled": true,
    "ssh.port": 2222,
    "ssh.version": "SSH-2.0-OpenSSH_5.1p1 Debian-4",
    "redis.enabled": false,
    "redis.port": 6379,
    "rdp.enabled": false,
    "rdp.port": 3389,
    "sip.enabled": false,
    "sip.port": 5060,
    "snmp.enabled": false,
    "snmp.port": 161,
    "ntp.enabled": false,
    "ntp.port": "123",
    "tftp.enabled": false,
    "tftp.port": 69,
    "tcpbanner.maxnum":10,
    "tcpbanner.enabled": false,
    "tcpbanner_1.enabled": false,
    "tcpbanner_1.port": 8001,
    "tcpbanner_1.datareceivedbanner": "",
    "tcpbanner_1.initbanner": "",
    "tcpbanner_1.alertstring.enabled": false,
    "tcpbanner_1.alertstring": "",
    "tcpbanner_1.keep_alive.enabled": false,
    "tcpbanner_1.keep_alive_secret": "",
    "tcpbanner_1.keep_alive_probes": 11,
    "tcpbanner_1.keep_alive_interval":300,
    "tcpbanner_1.keep_alive_idle": 300,
    "telnet.enabled": false,
    "telnet.port": "23",
    "telnet.banner": "",
    "telnet.honeycreds": [
        {
            "username": "admin",
            "password": "$pbkdf2-sha512$19000$bG1NaY3xvjdGyBlj7N37Xw$dGrmBqqWa1okTCpN3QEmeo9j5DuV2u1EuVFD8Di0GxNiM64To5O/Y66f7UASvnQr8.LCzqTm6awC8Kj/aGKvwA"
        },
        {
            "username": "admin",
            "password": "admin1"
        }
    ],
    "mssql.enabled": false,
    "mssql.version": "2012",
    "mssql.port":1433,
    "vnc.enabled": false,
    "vnc.port":5000
}
```
This file will not run in the home folder. You need to copy/paste it in the root home folder.
`cp ~/.opencanary.conf /root/.opencanary.conf`

To start opencanary
```
. env/bin/activate
opencanaryd --start
```

To start on boot create a file in `nano /etc/systemd/system/opencanary.service`

Copy and paste below in file
```
[Unit]
Description=OpenCanary honeypot
After=syslog.target
After=network.target

[Service]
User=pi
Restart=always
Environment=VIRTUAL_ENV=/home/pi/canary-env/
Environment=PATH=$VIRTUAL_ENV/bin:/usr/bin:$PATH
WorkingDirectory=/home/pi/canary-env/bin
ExecStart=/home/pi/canary-env/bin/opencanaryd --dev

[Install]
WantedBy=multi-user.target
```

Enable and start service
```
systemctl enable opencanary.service
systemctl start opencanary.service
```

Check your email to see if it start, you should have messages.

## Troubleshooting
***
The tool JQ can be used to check that the config file is well-formed JSON.

`jq . ~/.opencanary.conf`

Run opencanaryd in the foreground to see more error messages.

`opencanaryd --dev`

You may also easily restart the service using,

`opencanaryd --restart`

