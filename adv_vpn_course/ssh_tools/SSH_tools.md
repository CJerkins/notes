Some unique ssh tools you may like

# Autossh
autossh is a program to start a copy of ssh and monitor it, restarting it as necessary should it die or stop passing traffic. 

## Install
for mac `brew install autossh`
linux debian/ubuntu `apt install autossh`
linux RHEL/Centos `yum install autossh` `dnf install autossh`

## Use

usage: autossh [-V] [-M monitor_port[:echo_port]] [-f] [SSH_OPTIONS]

Normal example `autossh root@192.168.1.1`

### AutoSSH and -M (monitoring port)
With -M AutoSSH will continuously send data back and forth through the pair of monitoring ports in order to keep track of an established connection. If no data is going through anymore, it will restart the connection. The specified monitoring and the port directly above (+1) must be free. The first one is used to send data and the one above to receive data on.

There are multiple methods the example below are the best and most pratical.

`autossh -M 0 -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3"`
-M = monitor port. 0 disable the port
-o = ssh options

# Mosh
Remote terminal application that allows roaming, supports intermittent connectivity, and provides intelligent local echo and line editing of user keystrokes.

Mosh is a replacement for interactive SSH terminals. It's more robust and responsive, especially over Wi-Fi, cellular, and long-distance links.

Mosh runs over a UDP connect rather that TCP and default is between 60000 and 61000
## Install
https://mosh.org/#getting
Supported on every platform

Mosh must be installed on remote device.

## Use
mosh chewbacca.norad.mil
All other options are simular to SSH

# Proxychains
REF https://linuxhint.com/proxychains-tutorial/
REF https://medium.com/swlh/proxying-like-a-pro-cccdc177b081
REF https://github.com/haad/proxychains

ProxyChains is a UNIX program, that hooks network-related libc functions in dynamically linked programs via a preloaded DLL and redirects the connections through SOCKS4a/5 or HTTP proxies.
Warning: this program works only on dynamically linked programs. also both proxychains and the program to call must use the same dynamic linker (i.e. same libc) 

## Install
Proxychains-4.2.0 are available with pkgsrc to everyone using it on Linux, NetBSD, FreeBSD, OpenBSD, DragonFlyBSD or Mac OS X. You just need to install pkgsrc-wip repository and run make install in a wip/proxychains directory.

You can find out more about pkgsrc on pkgsrc and about pkgsrc-wip on Pkgsrc-wip homepage

**Installing on Mac OS X with homebrew**

You can install current proxychains on Mac OS X with an homebrew. You have to download unofficial homebrew formula from to your BREW_HOME by default /usr/local/Library/Formula/ and run

`brew install proxychains`

## Use
**When to use it**
- When the only way to get "outside" from your LAN is through proxy server.
- To get out from behind restrictive firewall which filters outgoing ports.
- To use two (or more) proxies in chain:
   `like: your_host <--> proxy1 <--> proxy2 <--> target_host`
- To "proxify" some program with no proxy support built-in (like telnet)
- Access intranet from outside via proxy.
- To use DNS behind proxy.
**Some cool features**
- This program can mix different proxy types in the same chain
  `like: your_host <-->socks5 <--> http <--> socks4 <--> target_host`
- Different chaining options supported random order from the list ( user defined length of chain ). exact order (as they appear in the list ) dynamic order (smart exclude dead proxies from chain)
- You can use it with any TCP client application, even network scanners yes, yes - you can make portscan via proxy (or chained proxies) for example with Nmap scanner by fyodor (www.insecure.org/nmap).
  `proxychains nmap -sT -PO -p 80 -iR`  (find some webservers through proxy)
- You can use it with servers, like squid, sendmail, or whatever.
- DNS resolving through proxy.

**Configuration**

proxychains looks for configuration in the following order:
- SOCKS5 proxy port in environment variable ${PROXYCHAINS_SOCKS5} (if set, no further configuration will be searched)
- file listed in environment variable ${PROXYCHAINS_CONF_FILE} or provided as a -f argument to proxychains script or binary.
- ./proxychains.conf
- $(HOME)/.proxychains/proxychains.conf
- /etc/proxychains.conf
see more in **/etc/proxychains.conf**

**Basic usage**
Proxychains can be used 2 different ways. One is to run programs though a local socks5 port, or second setup in the config a chain of proxy servers to ofuscate you source ip.
REF https://www.youtube.com/watch?v=qsA8zREbt6g
REF https://www.youtube.com/watch?v=_CPxlzznv0U

# SFTP
SFTP, which stands for SSH File Transfer Protocol, or Secure File Transfer Protocol, is a separate protocol packaged with SSH that works in a similar way over a secure connection. The advantage is the ability to leverage a secure connection to transfer files and traverse the filesystem on both the local and remote system.

## Install 
Comes packed with OpenSSH
`apt install openssh`

## Use

**Connect**
Like ssh you connect by the following:
`sftp root@192.168.1.1`
if you need to use ssh options include `-o[option=env]`

**Navigating**
Same as every linux you can `cd`,`ls`,`pwd`

**Transfering**
Transferring Remote Files to the Local System

If we would like download files from our remote host, we can do so by issuing the following command:

`get remoteFile`
```
Fetching /home/demouser/remoteFile to remoteFile
/home/demouser/remoteFile                       100%   37KB  36.8KB/s   00:01
```
As you can see, by default, the “get” command downloads a remote file to a file with the same name on the local file system.

We can copy the remote file to a different name by specifying the name afterwards:

`get remoteFile localFile`

The “get” command also takes some option flags. For instance, we can copy a directory and all of its contents by specifying the recursive option:

`get -r someDirectory`

We can tell SFTP to maintain the appropriate permissions and access times by using the “-P” or “-p” flag:

`get -Pr someDirectory`

Transferring Local Files to the Remote System

Transferring files to the remote system is just as easily accomplished by using the appropriately named “put” command:

`put localFile`

Uploading localFile to /home/demouser/localFile
localFile                                     100% 7607     7.4KB/s   00:00

The same flags that work with “get” apply to “put”. So to copy an entire local directory, you can issue:

`put -r localDirectory`


# sshto

If you need a gui to help with complex ssh techniques checkout sshto https://github.com/vaniacer/sshto

