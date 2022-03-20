# SSH adv techniques
REF https://hackertarget.com/ssh-examples-tunnels/

The site referenced is a very complete list of techinques for the us of ssh. Here I'll only list a very that interest in getting comms.
## SSH Basics Review
The following ssh example command uses common parameters often seen when connecting to a remote SSH server.

`localhost:~$ ssh -v -p 22 -C neo@remoteserver`

-v : Print debug information, particularly helpful when debugging an authentication problem. Can be used multiple times to print additional information.
-p 22 : Specify which port to connect to on the remote SSH server. 22 is not required as this is the default, but if any other port is listening connect to it using the -p parameter. The listening port is configured in the sshd_config file using the Port 2222 format.
-C : Compression is enabled on the connection using this parameter. If you are using the terminal over a slow link or viewing lots of text this can speed up the connection as it will compress the data transferred on the fly.
neo@ : The string before the @ symbol denotes the username to authenticate with against the remote server. Leaving out the user@ will default to using the username of the account you are currently logged in to (~$ whoami). User can also be specified with the -l parameter.
remoteserver : The hostname ssh is connecting to, this can be a fully qualified domain name, an IP address or any host in your local machines hosts file. To connect to a host that resolves to both IPv4 and IPv6 you can specify parameter -4 or -6 to the command line so it resolves correctly.

Apart from remoteserver, each of the above parameters is optional.

**Copy Files over SSH with SCP**

The ssh client comes with two other very handy tools for moving files around over an encrypted ssh connection. The commands are scp and sftp. See examples below for basic usage. Note that many parameters for the ssh can be applied to these commands also.

`localhost:~$ scp mypic.png neo@remoteserver:/media/data/mypic_2.png`

In this example, the file mypic.png was copied to the remoteserver to file system location /media/data and renamed to mypic_2.png.

## 1. Proxy Traffic over SSH using SOCKS

The SSH Proxy feature has been placed at number 1 for good reason. It is more powerful than many users realise giving you access to any system that the remote server can reach, using almost any application. The ssh client can tunnel traffic over the connection using a SOCKS proxy server with a quick one liner. A key thing to understand is that traffic to the remote systems will have a source of the remote server. For example in a web server log file.

`localhost:~$ ssh -D 8888 user@remoteserver`
```
localhost:~$ netstat -pan | grep 8888
tcp        0      0 127.0.0.1:8888       0.0.0.0:*               LISTEN      23880/ssh
```
Here we start the socks proxy server running on TCP port 8888, the second command checks that the port is now listening. The 127.0.0.1 indicates the service is running on localhost only. We can use a slightly different command to listen on all interfaces including ethernet or wifi, this will allow other applications (browsers or other) on our network to connect to the ssh socks proxy service.

`localhost:~$ ssh -D 0.0.0.0:8888 user@remoteserver`

Now we can configure our browser to connect to the socks proxy. In Firefox select preferences | general | network settings. Add the IP address and the port for the browser to connect to.

![41052fa7ddf913afe296044e4abac374.png](../../../_resources/41052fa7ddf913afe296044e4abac374.png)

**SSH Socks Proxy with DNS**

Note the option at the bottom of the form to force browser DNS requests to also go over the socks proxy. If you are using the proxy to encrypt your web traffic on the local network you will definitely want to select this option so the DNS requests are also tunnelled over the SSH connection.

## 2. SSH Tunnel (port forward)
REF https://www.youtube.com/watch?v=N8f5zv9UUMI
In its simplest form an SSH tunnel opens a port on your local system that connects through to another port at the other end of the tunnel.

`localhost:~$ ssh  -L 9999:127.0.0.1:80 user@remoteserver`

Lets break down the -L parameter. Think of -L as the Local listening side. So in our example above the port 9999 is listening on localhost and port forwards through to port 80 on remoteserver, note that the 127.0.0.1 refers to localhost on the remote server!

Lets take it up a notch. In this following example the port that is listening can be connected to from other hosts on the local network.

localhost:~$ ssh  -L 0.0.0.0:9999:127.0.0.1:80 user@remoteserver

In these examples the port we are connecting is a listening web server. It could also be a proxy server or any other TCP service.

## 3. SSH Tunnel Forward to Secondary Remote host

We can use the same options seen above to have the tunnel connect to another service running on a secondary system from the remote server.

`localhost:~$ ssh  -L 0.0.0.0:9999:10.10.10.10:80 user@remoteserver`

In this example we are forwarding the tunnel from remoteserver to the web server running on 10.10.10.10. The traffic from remoteserver -> 10.10.10.10 is no longer within the ssh tunnel. The web server on 10.10.10.10 will see remoteserver as the source of the web requests.

## 4. SSH Reverse Tunnel

In this scenario we want to setup a listening port on the remote server that will connect back to a local port on our localhost (or other system).

`localhost:~$ ssh -v -R 0.0.0.0:1999:127.0.0.1:902 192.168.1.100 user@remoteserver`

With this ssh session established a connection to the remoteserver port 1999 will be forwarded to port 902 on our local client.

## 5. SSH Reverse Proxy

In this case we are establishing a SOCKS proxy with our ssh connection, however the proxy is listening at the remote server end. With connections to that remote socks proxy now emerging from the tunnel as traffic originating from our localhost. Requires OpenSSH version 7.6+.

`localhost:~$ ssh -v -R 0.0.0.0:1999 192.168.1.100 user@remoteserver`

Troubleshooting Remote SSH Tunnels

If you are having trouble getting the remote SSH options to work, check with netstat which interface the listening port is attached too. Even though we have specified 0.0.0.0 in the above examples, if GatewayPorts is set to no in the sshd_config then the listener will only bind to localhost (127.0.0.1).
```
Security Warning
Note that when you are opening tunnels and socks proxies you may be exposing internal network resources to untrusted networks (like the Internet!). This can be a serious security risk so ensure you understand what is listening and what it has access too.
```

## 6. Remote Packet Capture & View in Wireshark

I grabbed this one from our tcpdump examples. Use it for a remote packet capture with the results feeding directly into your local Wireshark GUI.

``~$ ssh root@remoteserver 'tcpdump -c 1000 -nn -w - not port 22' | wireshark -k -i -``

## 7. Mount remote SSH location as local folder with SSHFS

Using sshfs - an ssh filesystem client, we can mount a local directory to a remote location with all file interaction taking place over the encrypted ssh session.

`localhost:~$ apt install sshfs`

On Ubuntu and Debian based system we install the sshfs package and then mount the remote location.

`localhost:~$ sshfs user@remoteserver:/media/data ~/data/`

## 8. SSH Multiplex using ControlPath

By default when you have an existing connection to a remote server with ssh, a second connection using ssh or scp will establish a new session with the overhead of authentication. Using the ControlPath options we can have the existing session be used for all subsequent connections. This will speed things up significantly. It is noticeable even on a local network but even more so when connecting to remote resources.
```
Host remoteserver
        HostName remoteserver.example.org
        ControlMaster auto
        ControlPath ~/.ssh/control/%r@%h:%p
        ControlPersist 10m
```
ControlPath denotes a socket that is checked by new connections to see if there is an existing ssh session that can be used. The ControlPersist option above means even after you exit the terminal, the existing session will remain open for 10 minutes, so if you were to reconnect within that time you would use that existing socket. See the ssh_config man page for more information.

## 9. Stream Video over SSH using VLC + SFTP

Long time users of ssh and vlc (Video Lan Client) are not always of aware of this handy option for when you need to watch video over the network. Using the vlc option to File | Open Network Stream one can enter the location as a an sftp:// location. A prompt will appear for authentication details if password is required.
```
sftp://remoteserver//media/uploads/myvideo.mkv
```

## 10. Bouncing through jump hosts with ssh and -J

When network segmentation means you are jumping through multiple ssh hosts to get to a final destination network or host, this jump host shortcut might be just what you need. Requires OpenSSH version 7.3+.

`localhost:~$ ssh -J host1,host2,host3 user@host4.internal`

A key thing to understand here is that this is not the same as ssh host1 then user@host1:~$ ssh host2, the -J jump parameter uses forwarding trickery so that the localhost is establishing the session with the next host in the chain. So our localhost is authenticating with host4 in the above example; meaning our localhost keys are used and the session from localhost to host4 is encrypted end to end.

To use this ability in the ssh_config use the ProxyJump configuration option. If you regularly have to jump through multiple hosts; use the config file and your alias to host4 will save you a lot of time.

## 11. Run ssh in the background

`ssh -fqN 8080 ubuntu@1.2.3.4`

-f = fork - start a new process in the background
-q = quiet
-N = no command

To kill the process `ps aux | grep ssh` then `kill 12345`

## 12. SSH Local Port Forwarding

Sometimes, you need to access a service but can’t do that directly because of some type of restriction (firewall blocking the IP). By using SSH local port forwarding, you can set it up so that a port on your local computer forwards connections through a remote SSH server to the port on some other server that offers the service you want to access.

In other words, if your computer is A, your remote SSH server is B, and the resource you want to access is on C, you can access a local port on A and seamlessly connect to whatever resource is on C, even if A and C cannot directly communicate. If B can communicate with both, A and C can interact with a local port forward using B as a relay.

Use Cases:
- You have servers on a LAN that do not have internet access but need updates/installs
- You have servers on a different LAN that you routinely need to access
- You need to access a remote box but your access is blocked somewhere but you can ssh into another box
- You have software that connects to a remote server but a firewall is blocking access

Pros:
- Easy to by pass Firewalls that are blacklisting IP addresses
    Traffic is encrypted

Cons:
- Syntax can be confusing
- May not support all protocols that you want to use
- You need to control/maintain a separate service to serve as a middle hop (attribution, costs)

`ssh -L 2222:lasthop:22 ubuntu@middlehop -p 20022`
or
`ssh -L 2222:2.3.4.5:22 ubuntu@1.2.3.4 -p 20022 -i ~/.ssh/key.pem`

ssh_config
```
Host middlehop
    Hostname 1.2.3.4
    User ubuntu
    Port 20022
    IdentityFile ~/.ssh/key.pem
    LocalForward 2222 2.3.4.5:22
```

## SSH Remote Port Forwarding

We can also do the opposite of local port forwarding—give access to resources we have locally to a remote connection. Instead of us listening on a local port, calling out to the SSH server B, and having our traffic forwarded to C, we call out to server B and tell it to listen on a port and forward connections to that port down to us at A. Then, C or an end user can connect to that port on B and be forwarded down to our machine A.  For this lesson plan we will not use server C but rather an end user

Use Cases:

- You are in a hotel and can't port forward from the hotel router to your local resource- ie chat, file share, etc.
- You want to give a foreign force access to your resource but you do not want them to directly connect to your IP
- You have multiple foreign forces that need access to your resource but they all need geographically matching IP addresses

Pros:
- Don't need to port forward traffic on a router
- Traffic is encrypted
- End user never knows the true IP of your resource

Cons:
- Syntax can be confusing
- May not support all protocols that you want to use
- You need to control/maintain a separate service to serve as a middle hop (attribution, costs)
- You are opening a local resource up to the public internet.

`ssh -R 0.0.0.0:8080:localhost:80 ubuntu@1.2.3.4 -p 20022`
`ssh -R <ip to allow to connect remote vm>:<port to allow>:localhost:80 ubuntu@<remote vm> -p 20022`

Login to your remote VPS and add GatewayPorts yes to `/etc/ssh/sshd_config`.  This is necessary because it is a security risk to allow the SSH port forward to listen on a public interface.  Adding this line proves you know what you're trying to do. Don't forget to restart the SSH server afterward to load the configuration change.

ssh_config syntax
```
Host remotevps
    Hostname 1.2.3.4
    User ubuntu
    Port 20022
    IdentityFile ~/.ssh/key.pem
    RemoteForward 0.0.0.0:8888 localhost:80
	```