# Nebula mesh vpn
REF 
- https://github.com/slackhq/nebula
- https://lucasjhall.com/2020/02/16/nebula-getting-started/
- https://www.youtube.com/watch?v=umu1A4me-Lw

Scripts
- https://github.com/Trozz/ansible-nebula
- https://github.com/jievince/nebula-ansible

## What is Nebula

Nebula is a scalable overlay networking tool with a focus on performance, simplicity and security. It lets you seamlessly connect computers anywhere in the world. Nebula is portable, and runs on Linux, OSX, and Windows. (Also: keep this quiet, but we have an early prototype running on iOS). It can be used to connect a small number of computers, but is also able to connect tens of thousands of computers.

Nebula incorporates a number of existing concepts like encryption, security groups, certificates, and tunneling, and each of those individual pieces existed before Nebula in various forms. What makes Nebula different to existing offerings is that it brings all of these ideas together, resulting in a sum that is greater than its individual parts.


![130216e057fa46ae24edd9a89e477a9c.png](../../../_resources/130216e057fa46ae24edd9a89e477a9c.png)

## Technical Overview

Nebula is a mutually authenticated peer-to-peer software defined network based on the Noise Protocol Framework. Nebula uses certificates to assert a node's IP address, name, and membership within user-defined groups. Nebula's user-defined groups allow for provider agnostic traffic filtering between nodes. Discovery nodes allow individual peers to find each other and optionally use UDP hole punching to establish connections from behind most firewalls or NATs. Users can move data between nodes in any number of cloud service providers, datacenters, and endpoints, without needing to maintain a particular addressing scheme.

Nebula uses elliptic curve Diffie-Hellman key exchange, and AES-256-GCM in its default configuration.

Nebula was created to provide a mechanism for groups hosts to communicate securely, even across the internet, while enabling expressive firewall definitions similar in style to cloud security groups.

## Lets get started

REF https://arstechnica.com/gadgets/2019/12/how-to-set-up-your-own-nebula-mesh-vpn-step-by-step/

You will need a clould hosted or port forwarded server for the lighthouse node, a node (it can be a computer, router, or other server. This does not work with mobiles).

Nebula doesn't actually have an installer; it's just two bare command line tools in a tarball, regardless of your operating system. For that reason, we're not going to give operating system specific instructions here: the commands and arguments are the same on Linux, MacOS, or Windows. Just download the appropriate tarball from the Nebula [release](https://github.com/slackhq/nebula/releases) page, open it up (Windows users will need 7zip for this), and dump the commands inside wherever you'd like them to be.

### Certificate Authority configuration and key generation

The first thing you'll need to do is create a Certificate Authority using the nebula-cert program. Nebula, thankfully, makes this a mind-bogglingly simple process:
```
./nebula-cert ca -name "My Shiny Nebula Mesh Network"
```
Now let's create the network. Unlike the CA certificate, node certificates need to have the Nebula IP address for each node baked into them when they're created.  So stop for a minute and think about what subnet you'd like to use for your Nebula mesh. It should be a private subnet—so it doesn't conflict with any Internet resources you might need to use—and it should be an oddball one so that it won't conflict with any LANs you happen to be on.

Nice, round numbers like 192.168.0.x, 192.168.1.x, 192.168.254.x, and 10.0.0.x should be right out, as the odds are extremely good you'll stay at a hotel, friend's house, etc that uses one of those subnets. We went with 192.168.98.x—but feel free to get more random than that. Your lighthouse will occupy .1 on whatever subnet you choose, and you will allocate new addresses for nodes as you create their keys. Let's go ahead and set up keys for our lighthouse and nodes now:
```
./nebula-cert sign -name "lighthouse" -ip "172.31.98.1/24"
./nebula-cert sign -name "banshee" -ip "172.31.98.2/24"
./nebula-cert sign -name "locutus" -ip "172.31.98.3/24"
./nebula-cert sign -name "firefly" -ip "172.31.98.4/24"
./nebula-cert sign -name "junebug" -ip "172.31.98.5/24"
./nebula-cert sign -name "grasshopper" -ip "172.31.98.6/24"
./nebula-cert sign -name "beezee" -ip "172.31.98.7/24"
./nebula-cert sign -name "bumblebee" -ip "172.31.98.8/24"
./nebula-cert sign -name "mothman" -ip "172.31.98.9/24"
./nebula-cert sign -name "mantis" -ip "172.31.98.10/24"
```
Now that you've generated all your keys, consider getting them the heck out of your lighthouse, for security. You need the ca.key file only when actually signing new keys, not to run Nebula itself. Ideally, you should move ca.key out of your working directory entirely to a safe place—maybe even a safe place that isn't connected to Nebula at all—and only restore it temporarily if and as you need it. Also note that the lighthouse itself doesn't need to be the machine that runs nebula-cert—if you're feeling paranoid, it's even better practice to do CA stuff from a completely separate box and just copy the keys and certs out as you create them.

Each Nebula node does need a copy of ca.crt, the CA certificate. It also needs its own .key and .crt, matching the name you gave it above. You don't need any other node's key or certificate, though—the nodes can exchange them dynamically as needed—and for security best practice, you really shouldn't keep all the .key and .crt files in one place. (If you lose one, you can always just generate another that uses the same name and Nebula IP address from your CA later.)

### Configuring Nebula with config.yml

Nebula's Github repo offers a sample config.yml with pretty much every option under the sun and lots of comments wrapped around them, and we absolutely recommend anyone interested poke through it  see to all the things that can be done. However, if you just want to get things moving, it may be easier to start with a drastically simplified config that has nothing but what you need.

Lines that begin with a hashtag are commented out and not interpreted.

Below is an example config for the lighthouse server. Make notice to these changes.

### Lighthouse config

1. Cert location
``` ca: /opt/nebula/ca.crt
  cert: /opt/nebula/lighthouse.crt
  key: /opt/nebula/lighthouse.key
```
2. Static host map (You'll find the public ip in instance dashboard)
```
"172.31.98.1": ["54.71.69.240:4242"] 
```
4. am_lighthouse set to true
```
am_lighthouse: true
```
5. Host list is commented out or empty
6. Listen interface and port
```
listen:
  host: 0.0.0.0
  port: 4242
```
7. You need to config the response time for punchy incase you have a tough NAT to punch through.
```
#respond: true

# delays a punch response for misbehaving NATs, default is 1 second, respond must be true to take effect
#delay: 1s
```
(Scroll pass lighthouse config to see node config instructions)
Example Lighthouse config
```
# This is the nebula example configuration file. You must edit, at a minimum, the static_host_map, lighthouse, and firewall sections
# Some options in this file are HUPable, including the pki section. (A HUP will reload credentials from disk without affecting existing tunnels)

# PKI defines the location of credentials for this node. Each of these can also be inlined by using the yaml ": |" syntax.
pki:
  # The CAs that are accepted by this node. Must contain one or more certificates created by 'nebula-cert ca'
  ca: /Users/drok/Desktop/nebula/ca.crt
  cert: /Users/drok/Desktop/nebula/jed24.crt
  key: /Users/drok/Desktop/nebula/jed24.key
  #blocklist is a list of certificate fingerprints that we will refuse to talk to
  #blocklist:
  #  - c99d4e650533b92061b09918e838a5a0a6aaee21eed1d12fd937682865936c72

# The static host map defines a set of hosts with fixed IP addresses on the internet (or any network).
# A host can have multiple fixed IP addresses defined here, and nebula will try each when establishing a tunnel.
# The syntax is:
#   "{nebula ip}": ["{routable ip/dns name}:{routable port}"]
# Example, if your lighthouse has the nebula IP of 192.168.100.1 and has the real ip address of 100.64.22.11 and runs on port 4242:
static_host_map:
  "172.31.98.1": ["54.71.69.240:4242"] # you find the public ip in instance dashboard


lighthouse:
  # am_lighthouse is used to enable lighthouse functionality for a node. This should ONLY be true on nodes
  # you have configured to be lighthouses in your network
  am_lighthouse: true
  # serve_dns optionally starts a dns listener that responds to various queries and can even be
  # delegated to for resolution
  #serve_dns: false
  #dns:
    # The DNS host defines the IP to bind the dns listener to. This also allows binding to the nebula node IP.
    #host: 0.0.0.0
    #port: 4242
  # interval is the number of seconds between updates from this node to a lighthouse.
  # during updates, a node sends information about its current IP addresses to each node.
  interval: 60
  # hosts is a list of lighthouse hosts this node should report to and query from
  # IMPORTANT: THIS SHOULD BE EMPTY ON LIGHTHOUSE NODES
  # IMPORTANT2: THIS SHOULD BE LIGHTHOUSES' NEBULA IPs, NOT LIGHTHOUSES' REAL ROUTABLE IPs
  hosts:

# - "172.31.98.1" # This is the your lighthouses private ip
# - "10.157.0.2" # This is the another lighthouse you report to

  # remote_allow_list allows you to control ip ranges that this node will
  # consider when handshaking to another node. By default, any remote IPs are
  # allowed. You can provide CIDRs here with `true` to allow and `false` to
  # deny. The most specific CIDR rule applies to each remote. If all rules are
  # "allow", the default will be "deny", and vice-versa. If both "allow" and
  # "deny" rules are present, then you MUST set a rule for "0.0.0.0/0" as the
  # default.
  remote_allow_list:
    # Example to block IPs from this subnet from being used for remote IPs.
    #"172.16.0.0/12": false

    # A more complicated example, allow public IPs but only private IPs from a specific subnet
    #"0.0.0.0/0": true
    #"10.0.0.0/8": false
    #"10.42.42.0/24": true

  # local_allow_list allows you to filter which local IP addresses we advertise
  # to the lighthouses. This uses the same logic as `remote_allow_list`, but
  # additionally, you can specify an `interfaces` map of regular expressions
  # to match against interface names. The regexp must match the entire name.
  # All interface rules must be either true or false (and the default will be
  # the inverse). CIDR rules are matched after interface name rules.
  # Default is all local IP addresses.
  #local_allow_list:
    # Example to block tun0 and all docker interfaces.
    #interfaces:
      #tun0: false
      #'docker.*': false
    # Example to only advertise this subnet to the lighthouse.
    #"10.0.0.0/8": true

# Port Nebula will be listening on. The default here is 4242. For a lighthouse node, the port should be defined,
# however using port 0 will dynamically assign a port and is recommended for roaming nodes.
listen:
  host: 0.0.0.0
  port: 4242
  # Sets the max number of packets to pull from the kernel for each syscall (under systems that support recvmmsg)
  # default is 64, does not support reload
  #batch: 64
  # Configure socket buffers for the udp side (outside), leave unset to use the system defaults. Values will be doubled by the kernel
  # Default is net.core.rmem_default and net.core.wmem_default (/proc/sys/net/core/rmem_default and /proc/sys/net/core/rmem_default)
  # Maximum is limited by memory in the system, SO_RCVBUFFORCE and SO_SNDBUFFORCE is used to avoid having to raise the system wide
  # max, net.core.rmem_max and net.core.wmem_max
  #read_buffer: 10485760
  #write_buffer: 10485760

punchy:
  # Continues to punch inbound/outbound at a regular interval to avoid expiration of firewall nat mappings
  punch: true

  # respond means that a node you are trying to reach will connect back out to you if your hole punching fails
  # this is extremely useful if one node is behind a difficult nat, such as a symmetric NAT
  # Default is false
  #respond: true

  # delays a punch response for misbehaving NATs, default is 1 second, respond must be true to take effect
  #delay: 1s

# Cipher allows you to choose between the available ciphers for your network.
# IMPORTANT: this value must be identical on ALL NODES/LIGHTHOUSES. We do not/will not support use of different ciphers simultaneously!
#cipher: chachapoly

# Local range is used to define a hint about the local network range, which speeds up discovering the fastest
# path to a network adjacent nebula node.
#local_range: "172.16.0.0/24"

# sshd can expose informational and administrative functions via ssh this is a
#sshd:
  # Toggles the feature
  #enabled: true
  # Host and port to listen on, port 22 is not allowed for your safety
  #listen: 127.0.0.1:2222
  # A file containing the ssh host private key to use
  # A decent way to generate one: ssh-keygen -t ed25519 -f ssh_host_ed25519_key -N "" < /dev/null
  #host_key: ./ssh_host_ed25519_key
  # A file containing a list of authorized public keys
  #authorized_users:
    #- user: steeeeve
      # keys can be an array of strings or single string
      #keys:
        #- "ssh public key string"

# Configure the private interface. Note: addr is baked into the nebula certificate
tun:
  # When tun is disabled, a lighthouse can be started without a local tun interface (and therefore without root)
  disabled: false
  # Name of the device
  dev: nebula0
  # Toggles forwarding of local broadcast packets, the address of which depends on the ip/mask encoded in pki.cert
  drop_local_broadcast: false
  # Toggles forwarding of multicast packets
  drop_multicast: false
  # Sets the transmit queue length, if you notice lots of transmit drops on the tun it may help to raise this number. Default is 500
  tx_queue: 500
  # Default MTU for every packet, safe setting is (and the default) 1300 for internet based traffic
  mtu: 1300
  # Route based MTU overrides, you have known vpn ip paths that can support larger MTUs you can increase/decrease them here
  routes:
    #- mtu: 8800
    #  route: 10.0.0.0/16
  # Unsafe routes allows you to route traffic over nebula to non-nebula nodes
  # Unsafe routes should be avoided unless you have hosts/services that cannot run nebula
  # NOTE: The nebula certificate of the "via" node *MUST* have the "route" defined as a subnet in its certificate
  unsafe_routes:
    #- route: 172.16.1.0/24
    #  via: 192.168.100.99
    #  mtu: 1300 #mtu will default to tun mtu if this option is not sepcified


# TODO
# Configure logging level
logging:
  # panic, fatal, error, warning, info, or debug. Default is info
  level: info
  # json or text formats currently available. Default is text
  format: text
  # timestamp format is specified in Go time format, see:
  #     https://golang.org/pkg/time/#pkg-constants
  # default when `format: json`: "2006-01-02T15:04:05Z07:00" (RFC3339)
  # default when `format: text`:
  #     when TTY attached: seconds since beginning of execution
  #     otherwise: "2006-01-02T15:04:05Z07:00" (RFC3339)
  # As an example, to log as RFC3339 with millisecond precision, set to:
  #timestamp_format: "2006-01-02T15:04:05.000Z07:00"

#stats:
  #type: graphite
  #prefix: nebula
  #protocol: tcp
  #host: 127.0.0.1:9999
  #interval: 10s

  #type: prometheus
  #listen: 127.0.0.1:8080
  #path: /metrics
  #namespace: prometheusns
  #subsystem: nebula
  #interval: 10s

  # enables counter metrics for meta packets
  #   e.g.: `messages.tx.handshake`
  # NOTE: `message.{tx,rx}.recv_error` is always emitted
  #message_metrics: false

  # enables detailed counter metrics for lighthouse packets
  #   e.g.: `lighthouse.rx.HostQuery`
  #lighthouse_metrics: false

# Handshake Manger Settings
#handshakes:
  # Total time to try a handshake = sequence of `try_interval * retries`
  # With 100ms interval and 20 retries it is 23.5 seconds
  #try_interval: 100ms
  #retries: 20
  # wait_rotation is the number of handshake attempts to do before starting to try non-local IP addresses
  #wait_rotation: 5
  # trigger_buffer is the size of the buffer channel for quickly sending handshakes
  # after receiving the response for lighthouse queries
  #trigger_buffer: 64

# Nebula security group configuration
firewall:
  conntrack:
    tcp_timeout: 12m
    udp_timeout: 3m
    default_timeout: 10m
    max_connections: 100000

  # The firewall is default deny. There is no way to write a deny rule.
  # Rules are comprised of a protocol, port, and one or more of host, group, or CIDR
  # Logical evaluation is roughly: port AND proto AND (ca_sha OR ca_name) AND (host OR group OR groups OR cidr)
  # - port: Takes `0` or `any` as any, a single number `80`, a range `200-901`, or `fragment` to match second and further fragments of fragmented packets (since there is no port available).
  #   code: same as port but makes more sense when talking about ICMP, TODO: this is not currently implemented in a way that works, use `any`
  #   proto: `any`, `tcp`, `udp`, or `icmp`
  #   host: `any` or a literal hostname, ie `test-host`
  #   group: `any` or a literal group name, ie `default-group`
  #   groups: Same as group but accepts a list of values. Multiple values are AND'd together and a certificate would have to contain all groups to pass
  #   cidr: a CIDR, `0.0.0.0/0` is any.
  #   ca_name: An issuing CA name
  #   ca_sha: An issuing CA shasum

  outbound:
    # Allow all outbound traffic from this node
    - port: any
      proto: any
      host: any

  inbound:
    # Allow icmp between any nebula hosts
    - port: any
      proto: icmp
      host: any

    # Allow tcp/443 from any host with BOTH laptop and home group
#    - port: 443
#      proto: tcp
#      groups:
#        - laptop
#        - home
```
### Node config
The Node the config is almost exactly the same except for a few differences.

1. Cert location (Each nodes cert)
``` ca: /opt/nebula/ca.crt
  cert: /opt/nebula/banshee.crt
  key: /opt/nebula/banshee.key
```
3. am_lighthouse set to false
```
am_lighthouse: false
```
3. Host list should be lighthouse node IPs, not lighthouses real routable IPs
```
  hosts:
    - "172.31.98.1"
```
4. Listen interface and port (Set port to '0' for random port assignment. This is for roaming nodes)
```
listen:
  host: 0.0.0.0
  port: 0
```
5. You need to config the response time for punchy incase you have a tough NAT to punch through.
```
#respond: true

# delays a punch response for misbehaving NATs, default is 1 second, respond must be true to take effect
#delay: 1s
```

(Scroll pass node config to see node config instructions)
Example node config
```
# This is the nebula example configuration file. You must edit, at a minimum, the static_host_map, lighthouse, and firewall sections
# Some options in this file are HUPable, including the pki section. (A HUP will reload credentials from disk without affecting existing tunnels)

# PKI defines the location of credentials for this node. Each of these can also be inlined by using the yaml ": |" syntax.
pki:
  # The CAs that are accepted by this node. Must contain one or more certificates created by 'nebula-cert ca'
  ca: /Users/drok/Desktop/nebula/ca.crt
  cert: /Users/drok/Desktop/nebula/jed24.crt
  key: /Users/drok/Desktop/nebula/jed24.key
  #blocklist is a list of certificate fingerprints that we will refuse to talk to
  #blocklist:
  #  - c99d4e650533b92061b09918e838a5a0a6aaee21eed1d12fd937682865936c72

# The static host map defines a set of hosts with fixed IP addresses on the internet (or any network).
# A host can have multiple fixed IP addresses defined here, and nebula will try each when establishing a tunnel.
# The syntax is:
#   "{nebula ip}": ["{routable ip/dns name}:{routable port}"]
# Example, if your lighthouse has the nebula IP of 192.168.100.1 and has the real ip address of 100.64.22.11 and runs on port 4242:
static_host_map:
  "172.31.98.1": ["54.71.69.240:4242"] # you find the public ip in instance dashboard


lighthouse:
  # am_lighthouse is used to enable lighthouse functionality for a node. This should ONLY be true on nodes
  # you have configured to be lighthouses in your network
  am_lighthouse: true
  # serve_dns optionally starts a dns listener that responds to various queries and can even be
  # delegated to for resolution
  #serve_dns: false
  #dns:
    # The DNS host defines the IP to bind the dns listener to. This also allows binding to the nebula node IP.
    #host: 0.0.0.0
    #port: 53
  # interval is the number of seconds between updates from this node to a lighthouse.
  # during updates, a node sends information about its current IP addresses to each node.
  interval: 60
  # hosts is a list of lighthouse hosts this node should report to and query from
  # IMPORTANT: THIS SHOULD BE EMPTY ON LIGHTHOUSE NODES
  # IMPORTANT2: THIS SHOULD BE LIGHTHOUSES' NEBULA IPs, NOT LIGHTHOUSES' REAL ROUTABLE IPs
 hosts:
    - "172.31.98.1"
	
  # remote_allow_list allows you to control ip ranges that this node will
  # consider when handshaking to another node. By default, any remote IPs are
  # allowed. You can provide CIDRs here with `true` to allow and `false` to
  # deny. The most specific CIDR rule applies to each remote. If all rules are
  # "allow", the default will be "deny", and vice-versa. If both "allow" and
  # "deny" rules are present, then you MUST set a rule for "0.0.0.0/0" as the
  # default.
  remote_allow_list:
    # Example to block IPs from this subnet from being used for remote IPs.
    #"172.16.0.0/12": false

    # A more complicated example, allow public IPs but only private IPs from a specific subnet
    #"0.0.0.0/0": true
    #"10.0.0.0/8": false
    #"10.42.42.0/24": true

  # local_allow_list allows you to filter which local IP addresses we advertise
  # to the lighthouses. This uses the same logic as `remote_allow_list`, but
  # additionally, you can specify an `interfaces` map of regular expressions
  # to match against interface names. The regexp must match the entire name.
  # All interface rules must be either true or false (and the default will be
  # the inverse). CIDR rules are matched after interface name rules.
  # Default is all local IP addresses.
  #local_allow_list:
    # Example to block tun0 and all docker interfaces.
    #interfaces:
      #tun0: false
      #'docker.*': false
    # Example to only advertise this subnet to the lighthouse.
    #"10.0.0.0/8": true

# Port Nebula will be listening on. The default here is 4242. For a lighthouse node, the port should be defined,
# however using port 0 will dynamically assign a port and is recommended for roaming nodes.
listen:
  host: 0.0.0.0
  port: 4242
  # Sets the max number of packets to pull from the kernel for each syscall (under systems that support recvmmsg)
  # default is 64, does not support reload
  #batch: 64
  # Configure socket buffers for the udp side (outside), leave unset to use the system defaults. Values will be doubled by the kernel
  # Default is net.core.rmem_default and net.core.wmem_default (/proc/sys/net/core/rmem_default and /proc/sys/net/core/rmem_default)
  # Maximum is limited by memory in the system, SO_RCVBUFFORCE and SO_SNDBUFFORCE is used to avoid having to raise the system wide
  # max, net.core.rmem_max and net.core.wmem_max
  #read_buffer: 10485760
  #write_buffer: 10485760

punchy:
  # Continues to punch inbound/outbound at a regular interval to avoid expiration of firewall nat mappings
  punch: true

  # respond means that a node you are trying to reach will connect back out to you if your hole punching fails
  # this is extremely useful if one node is behind a difficult nat, such as a symmetric NAT
  # Default is false
  #respond: true

  # delays a punch response for misbehaving NATs, default is 1 second, respond must be true to take effect
  #delay: 1s

# Cipher allows you to choose between the available ciphers for your network.
# IMPORTANT: this value must be identical on ALL NODES/LIGHTHOUSES. We do not/will not support use of different ciphers simultaneously!
#cipher: chachapoly

# Local range is used to define a hint about the local network range, which speeds up discovering the fastest
# path to a network adjacent nebula node.
#local_range: "172.16.0.0/24"

# sshd can expose informational and administrative functions via ssh this is a
#sshd:
  # Toggles the feature
  #enabled: true
  # Host and port to listen on, port 22 is not allowed for your safety
  #listen: 127.0.0.1:2222
  # A file containing the ssh host private key to use
  # A decent way to generate one: ssh-keygen -t ed25519 -f ssh_host_ed25519_key -N "" < /dev/null
  #host_key: ./ssh_host_ed25519_key
  # A file containing a list of authorized public keys
  #authorized_users:
    #- user: steeeeve
      # keys can be an array of strings or single string
      #keys:
        #- "ssh public key string"

# Configure the private interface. Note: addr is baked into the nebula certificate
tun:
  # When tun is disabled, a lighthouse can be started without a local tun interface (and therefore without root)
  disabled: false
  # Name of the device
  dev: nebula0
  # Toggles forwarding of local broadcast packets, the address of which depends on the ip/mask encoded in pki.cert
  drop_local_broadcast: false
  # Toggles forwarding of multicast packets
  drop_multicast: false
  # Sets the transmit queue length, if you notice lots of transmit drops on the tun it may help to raise this number. Default is 500
  tx_queue: 500
  # Default MTU for every packet, safe setting is (and the default) 1300 for internet based traffic
  mtu: 1300
  # Route based MTU overrides, you have known vpn ip paths that can support larger MTUs you can increase/decrease them here
  routes:
    #- mtu: 8800
    #  route: 10.0.0.0/16
  # Unsafe routes allows you to route traffic over nebula to non-nebula nodes
  # Unsafe routes should be avoided unless you have hosts/services that cannot run nebula
  # NOTE: The nebula certificate of the "via" node *MUST* have the "route" defined as a subnet in its certificate
  unsafe_routes:
    #- route: 172.16.1.0/24
    #  via: 192.168.100.99
    #  mtu: 1300 #mtu will default to tun mtu if this option is not sepcified


# TODO
# Configure logging level
logging:
  # panic, fatal, error, warning, info, or debug. Default is info
  level: info
  # json or text formats currently available. Default is text
  format: text
  # timestamp format is specified in Go time format, see:
  #     https://golang.org/pkg/time/#pkg-constants
  # default when `format: json`: "2006-01-02T15:04:05Z07:00" (RFC3339)
  # default when `format: text`:
  #     when TTY attached: seconds since beginning of execution
  #     otherwise: "2006-01-02T15:04:05Z07:00" (RFC3339)
  # As an example, to log as RFC3339 with millisecond precision, set to:
  #timestamp_format: "2006-01-02T15:04:05.000Z07:00"

#stats:
  #type: graphite
  #prefix: nebula
  #protocol: tcp
  #host: 127.0.0.1:9999
  #interval: 10s

  #type: prometheus
  #listen: 127.0.0.1:8080
  #path: /metrics
  #namespace: prometheusns
  #subsystem: nebula
  #interval: 10s

  # enables counter metrics for meta packets
  #   e.g.: `messages.tx.handshake`
  # NOTE: `message.{tx,rx}.recv_error` is always emitted
  #message_metrics: false

  # enables detailed counter metrics for lighthouse packets
  #   e.g.: `lighthouse.rx.HostQuery`
  #lighthouse_metrics: false

# Handshake Manger Settings
#handshakes:
  # Total time to try a handshake = sequence of `try_interval * retries`
  # With 100ms interval and 20 retries it is 23.5 seconds
  #try_interval: 100ms
  #retries: 20
  # wait_rotation is the number of handshake attempts to do before starting to try non-local IP addresses
  #wait_rotation: 5
  # trigger_buffer is the size of the buffer channel for quickly sending handshakes
  # after receiving the response for lighthouse queries
  #trigger_buffer: 64

# Nebula security group configuration
firewall:
  conntrack:
    tcp_timeout: 12m
    udp_timeout: 3m
    default_timeout: 10m
    max_connections: 100000

  # The firewall is default deny. There is no way to write a deny rule.
  # Rules are comprised of a protocol, port, and one or more of host, group, or CIDR
  # Logical evaluation is roughly: port AND proto AND (ca_sha OR ca_name) AND (host OR group OR groups OR cidr)
  # - port: Takes `0` or `any` as any, a single number `80`, a range `200-901`, or `fragment` to match second and further fragments of fragmented packets (since there is no port available).
  #   code: same as port but makes more sense when talking about ICMP, TODO: this is not currently implemented in a way that works, use `any`
  #   proto: `any`, `tcp`, `udp`, or `icmp`
  #   host: `any` or a literal hostname, ie `test-host`
  #   group: `any` or a literal group name, ie `default-group`
  #   groups: Same as group but accepts a list of values. Multiple values are AND'd together and a certificate would have to contain all groups to pass
  #   cidr: a CIDR, `0.0.0.0/0` is any.
  #   ca_name: An issuing CA name
  #   ca_sha: An issuing CA shasum

  outbound:
    # Allow all outbound traffic from this node
    - port: any
      proto: any
      host: any

  inbound:
    # Allow icmp between any nebula hosts
    - port: any
      proto: icmp
      host: any

    # Allow tcp/443 from any host with BOTH laptop and home group
#    - port: 443
#      proto: tcp
#      groups:
#        - laptop
#        - home
```

There isn't much different between lighthouse and normal node configs. If the node is not to be a lighthouse, just set am_lighthouse to false, and uncomment (remove the leading hashtag from) the line # - "192.168.98.1", which points the node to the lighthouse it should report to.

Note that the lighthouse:hosts list uses the nebula IP of the lighthouse node, not its real-world public IP! The only place real-world IP addresses should show up is in the static_host_map section.
Starting nebula on each node

I hope you Windows and Mac types weren't expecting some sort of GUI—or an applet in the dock or system tray, or a preconfigured service or daemon—because you're not getting one. Grab a terminal—a command prompt run as Administrator, for you Windows folks—and run nebula against its config file. Minimize the terminal/command prompt window after you run it.
```
./nebula -config ./config.yml
```

Thats it, easy.