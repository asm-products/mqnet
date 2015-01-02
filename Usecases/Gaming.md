# Setup

Group of people plays old LAN game through MQNet.

There are

* 4 players total
* Every player is behind NAT
* Traffic is not encrypted
* Traffic is not compressed
* All computers are Windows based except **Hub computer** which is GNU/Linux

# Hub computer

Hub computer is temporarily rented virtual machine from some hosting company running at IP `1.2.3.4/32`

* MQNet virtual wire is running on port `4321` at `eth0` 
* Hub computer is running simple UDP repeater application written for example in python on ports `67` and `68` (DHCP) on `eth0:0` alias interface on network `192.168.0.0/24` and is configured on virtual wire channel named "`old-lan-games`" and is connected to `1.2.3.4:4321` 
* `dnsmasq` is set up as DHCP server on `eth0:1` alias interface on network `192.168.0.0/24`
* `eth0:0` and `eth0:1` are bridged together with `brctl`
* => Hub computer distributes IPs to virtual wire connected computers

# Gaming computers

* Virtual NIC / VPN settings are:
  * IP/host: `1.2.3.4`
  * Port: `4321`
  * Channel: `old-lan-games`
* Computers get `192.168.0.0/24` IP addresses from dnsmasq DHCP server listening in virtual wire

# Next possible scenario: add compression module

Players find that there is too much packet fragmentation and compression might help that

* They install MQNet compression module to all machines which uses for example LZO
* Settings for adapters and applications/modules are then:
  * IP/host: `localhost`
  * Port: `11111`
  * Channel: `my-computer`
* Compression module settings are:
  * Receive port: `11111`
  * Receive channel: `my-computer`
  * Server IP/host: `1.2.3.4`
  * Server Port: `4321`
  * Server Channel: `old-lan-games`
* => raw ethernet packets are now sent to virtual wire compressed with LZO 

# Questions

Should every virtual wire using "module" add some kind of meta information which could be used to ask different machines what they are running? 

Pros:

* You could run for example that DHCP traffic uncompressed and gaming data compressed
* No need to install all modules to every machine
* Possibility to ask virtual wire clients for example that they are running latest version of module.

Cons:

* Some more overhead but usually only asked on new connection only 
* Possible security risks that someone is not running for example encryption module (but this can be fixed with that new connection meta information channel on virtual wire)

# Going even further
There could be some kind of registration channel on the virtual wire which asks what modules user has installed and also might install missing or update modules.