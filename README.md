# MQNet

Open virtual networks to developers

* VPN or vNIC?
* OpenVPN 3 ideas: https://community.openvpn.net/openvpn/wiki/RoadMap#OpenVPN3.0:Designandimplementation
* zMQ - http://zeromq.org/intro:read-the-manual
* Push raw packets to MQ virtual wire
* UDP MQ wire socket for UDP packets, TCP for TCP packets
* In adapter settings:
  * zMQ IP
  * zMQ port
  * Channel UUID
* Out-of-box p2p/multi user:
  * Users decide which one is server and use that user's IP, port and channel
  * Not encrypted
  * Not compressed
* Able to chain virtual wires
  * Physical NIC -> raw data to zMQ wire -> compressing wire -> crypting wire
  * Each virtual wire can be running in different machine, port and channel
* zMQ can automatically distribute work among many machines (distributed firewalls, http proxies, ..)
