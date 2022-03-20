Usage: openvpn-generate [-3if] [-d directives_file] [-n clients] [-e easyrsa_script ] [-o output_dir ] -c common_name

This script requires OpenVPN to be installed as well as the EasyRSA script which can be obtained via git: https://github.com/OpenVPN/easy-rsa.git
Generates OpenVPN configs compatible with OpenVPN 2.4 by default (tls-crypt).

-3: Generates OpenVPN XML configs compatible with OpenVPN 2.3 (tls-auth and key-direction).
-f: Forces overwrite and CN confirmations to yes.
-i: Insecure mode (CA private key will be temporarily decrypted so you only have to enter the CA password once).
-d: Specify a directives file in the same format as the directives.example file provided. Looks for "directives.$common_name" if not specified.
-n: Specify number of subnet client configs to generate, or 0 to generate a p2p client. Defaults to p2p if not specified.
  If common_name already exists, -n will create more subnet clients if some already exist.
-e: Specify location of easyrsa script. Defaults to current directory if not specified.
-o: Specify output location of completed config files. Defaults to ./$common_name/conf if not specified. 
-r: Generate an installer script at the top of the output files.

`opnvpn-generate -d directives.examples -n 6 -o unix -c unix`

```
##Example directives file for openvpn-generate script.
# DEV_TYPE should be either "tun" or "tap"
DEV_TYPE="tun"

# DEV_NAME_SERVER should be set to the name (max 15 characters) you want to give the tun/tap device on the server side.
DEV_NAME_SERVER="gw0"

#DEV_NAME_CLIENT is only useful for p2p clients so is ignored for subnet clients.
DEV_NAME_CLIENT="vpngw0"

#PROTO can be udp, tcp-server, tcp-client
PROTO="udp"

#PORT is the port the server listens on and client connects to.
PORT="2930"

#IFCONFIG_SERVER and IFCONFIG_CLIENT are the IP addresses for either side of a p2p tunnel and are ignored for subnet topologies.
IFCONFIG_SERVER="10.50.0.1"
IFCONFIG_CLIENT="10.50.0.2"

#SERVER is the network ID and subnet mask the subnet topology VPN server should use, and is ignored for p2p topologies.
#SERVER="10.100.0.0 255.255.255.0"

#REMOTE is the IP address of the server side of the VPN. TODO: Allow multiple remote lines like "1.2.3.4 1194 udp,2.3.4.5 1195 tcp-client"
REMOTE="54.167.70.167"

#REDIRECT_GATEWAY set to yes will put 'push "redirect-gateway def1"' into subnet servers. If you set REDIRECT_GATEWAY="yes" then REDIRECT_PRIVATE should be "no"
REDIRECT_GATEWAY="no"

#REDIRECT_PRIVATE will put 'push "redirect-private"' into p2p servers. This will tell the p2p client to add a /32 route directly to the p2p server public IP and is useful in certain situations. If you set REDIRECT_PRIVATE="yes" then REDIRECT_GATEWAY should be "no"
REDIRECT_PRIVATE="yes"

#LOG can be yes or no and will create a log in /etc/openvpn/ for subnet servers and p2p servers/clients, but is ignored for subnet clients. Log verbosity set to 4.
LOG="yes"
VERB="4"

#Tell VPN tunnels to send a ping every 10 seconds and restart the tunnel if no response within 60 seconds
KEEPALIVE="10 60"

#DNS servers can be specified separated by commas, as many as you want
#Comment this line if you don't need to push DNS servers or set it to an empty string
#DNS="172.16.50.2,8.8.8.8"

#Push a domain suffix to clients connecting on subnet VPNs
#Comment this line if you don't need to push a domain suffix or set it to an empty string
DOMAIN="vanguard.net"

#Routes can be specified separated by commas, either in the "ip netmask" format or "ip netmask gateway" format.
#Comment this line if you don't need to push routes or set it to an empty string
#ROUTES="10.0.1.2 255.255.255.255,10.0.2.2 255.255.255.255,10.0.3.2 255.255.255.255 127.0.0.1"

#BLOCK_CLIENT_DNS set to yes will put 'push "block-outside-dns"' in subnet server configs.
BLOCK_CLIENT_DNS="yes"

#Hardening directives below
#OpenVPN 2.4 by default will automatically negotiate the highest possible cipher between servers and clients (AES-256-GCM). If you want to specify a lesser cipher, set NCP_DISABLE to yes. Ignored for 2.3 configs
NCP_DISABLE="yes"

#CIPHER is the encryption cipher. Change the cipher from GCM to CBC if using OpenVPN 2.3. (automatic)
CIPHER="AES-256-GCM"

#TLS_ELLIPTIC_CURVE is only available on OpenVPN 2.4, so set TLS_DH_CIPHER for 2.3 configs and TLS_EC_CIPHER for 2.4 configs. You can turn TLS_ELLIPTIC_CURVE off if you need 2.3 clients to connect to a 2.4 server.
TLS_ELLIPTIC_CURVE="yes"
TLS_DH_CIPHER="TLS-DHE-RSA-WITH-AES-256-GCM-SHA384"
TLS_EC_CIPHER="TLS-ECDHE-RSA-WITH-AES-256-GCM-SHA384"

#The following line may be useful if you are using OpenVPN <=2.3.3
#TLS_DH_CIPHER="TLS-DHE-RSA-WITH-AES-256-CBC-SHA"

#AUTH is SHA1 by default but set to SHA256 here
AUTH="SHA256"

#VERIFY_X509_NAME, REMOTE_CERT_TLS, CHROOT, and DROP_PRIVILEGES are OpenVPN hardening options.
VERIFY_X509_NAMES="yes"
REMOTE_CERT_TLS="yes"

#If you use CHROOT="yes" create a directory called $common_name-jail and $common_name-jail/tmp and chmod the tmp directory 1777
CHROOT="yes"

#Drop root privileges after starting.
DROP_PRIVILEGES="yes"
UNPRIVILEGED_USER="nobody"
UNPRIVILEGED_GROUP="nogroup"
```