
IPs ending in zeros are not actual hosts, but indicate the start of a block (since real IP addresses end in a number between 1 and 254)
- 255 reserved for masking

### Subnet
- a subnet is a logical division of an IP network
	- an IP network is any network that uses IP address to communicate to other devices.
		- ex. LAN, the internet, enterprise network.
	- you might do this for logical reasons (ex. firewalling) for physical reasons (ex. smaller broadcast domains)
- to be useful, a router is connected to two or more IP subnets
- IP subnets exist to allow routers to choose appropriate destinations for packets.
- Traffic is exchanged between subnetworks through routers
- subnetting is the process of breaking down a single IP address block into smaller subnetworks called subnets
	- the reason we need to subnet is to efficiently distribute IP addresses to reduce wastage
- the IP address is made up of 2 parts: the subnet number and the host identifier.
	- the number of bit-groups that are taken up by the subnet number depends on how the subnet mask is defined.
		- ex. if the subnet mask is `255.255.255.0`, then the subnet number consists of the first 3 groups. If it is `255.255.0.0`, then it consists of the first 2 groups
- subnetting is analogous to the concept of *zoning* in city planning

#### Subnet Mask
- a subnet mask allows a computer/router to determine the portion of the IP address which refers to the network, and the portion that refers to the host machine
	- the network portion of an address is represented by 1s (255) in the subnet mask, and the host portion is represented by 0s
	- a subnet mask can also tell us the number of hosts within a network
	- anal. just like our home address consists of a street name (network ID) and a number (network host), the mask's job is to determine where in the IP address one begins and the other ends.
- each octet of a subnet mask can either be 255 or 0. When the octet on the mask is 255, that means that the when trying to connect to another node, it is going to go through the router to try and resolve that IP address
	- ex. if the host has IP=`168.25.4.6` and mask=`255.255.255.0`, that means that it will only try and hit nodes within the LAN if the IP address starts with `168.25.4`. If the host tried to connect to `168.25.8.2`, it would default to going through the gateway (router)
		- If the mask=`255.255.0.0`, that means the host will try to hit the node locally only if the IP address starts with `168.25`
		- if at any point the 255 is "triggered" (ie. the external node doesn't satisfy the mask's requirement for enabling local searching), then the external node is said to be "outside the mask"
- When we apply the subnet mask to the IP address, we get the *routing prefix*
- ex. we have a network with IP=`135.68.2.0`. In that network are 2 host computers with IP=`135.68.2.1` and IP=`135.68.2.2`. The network portion of each host's IP is `135.68.2`
	-  a 0 at the end of an IP address indicates that it is a network address
- anal. house addresses are composed of a streetname and a number
- with a mask of 255.255.255.0, since only one of the octets of bits refers to the hosts in a network, there can only be 256 nodes within that network
	- if the mask were 255.255.0.0, there could be 65,536
	- in reality we have to subtract 2 from that total, since there are 2 reserved IP addresses: the Network ID and the Broadcast IP address.
- a subnet mask is needed to tell us how many computers a node within a subnet has to go through before it gets to another node on that same network (ex. the computers at a LAN party).
	- When trying to send a message across the network, the subnet mask will tell us if we can access that node via the current network, or if we can only connect to it through the router (ie. it is on the internet)
- Another way of looking at the subnet mask is that it tells us which octets of an IP address are devoted to telling us which network the nodes are located in.
	- when a mask is 255.255.255.0, then anything is on the same network as the host if the first 3 octets of the IP address are identical.

*Default Subnet Masks*
- There are 3 classes:
Class A - 255.0.0.0
Class B - 255.255.0.0
Class C - 255.255.255.0

note: the concept of default subnet masks is becoming less relevant with the adoption of [[CIDR|network.internet.ip#cidr-classless-inter-domain-routing]]

### Modem
- portmanteau of "modulator-demodulator"
- purpose is to convert data from a digital format to a format that is conducive to transmission over a physical layer
- Modems can be used with almost any means of transmitting analog signals, from light-emitting diodes to radio
- Any communication technology sending digital data wirelessly involves a modem.
	- ex. satellite, WiFi, WiMax, mobile phones, GPS, Bluetooth and NFC.

### Demilitarized Zone (DMZ)
- a subnetwork that sits between the network and the router. the DMZ exists so that we can control exposure to certain parts of a network. Anything in it is exposed to untrusted networks (like the internet). The idea is that the DMZ is all that can be accessed externally, so the purpose is added security

### Dynamic Host Configuration Protocol (DHCP)
DHCP is a server hosted within a network that dynamically assigns IP addresses
- it is therefore the service that manages the IP address pool in a network

**DHCP Reservation** - set aside an IP address and map it to a specific MAC address. Whenever a device with the specified MAC address enters the network, it is assigned with the specified IP address.

#### Static IP
- on the device receiving IP assignment (ie. not the router), we can bypass the dynamic assigning of IP addresses by creating a entry in `/etc/dhcpcd.conf` (Linux)
- Generally accepted best practice is to assign a static address on the device that needs it - ex. a NAS - rather than rely on a DHCP server to give you the address you are expecting.
	- This shows that there are 2 ways to achieve a predictable IP address for devices on a LAN

##### Linux approach
run `hostname -I`
append `ip=YOURIP` to end of `/boot/cmdline.txt`

### Localhost vs LAN
- localhost (an alias for `127.0.0.1`) is an IP address that is used to test the computer's networking protocols without actually using the LAN that the computer is attached to.
	- Called a loopback address, and it is analogous to hooking up an outbound cable out of one end of a machine and into the other (as opposed to that cable being hooked up to a router).
- On the other hand, the private IP (`192.168.X.X`) is created by your network (the router), and allows us to communicate with other devices in the same network.
- anal. Imagine there were 2 postal services: 1 for your street (local), and one for the whole world (global). When you write a letter, you give it to your local postman, and he connects it to the global postal network. If you are sending your letter to the localhost network, then it is like writing a letter and handing it to yourself. If you are sending your letter to the `192.168` address, then it is like putting it in a mailbox, where your local postman proceeds to deliver it to you.
	- In this analogy it's important to note that the letter never reaches the global post network (ie the internet).

## Routing Schemes
### Broadcast
transfer a message to all recipients simultaneously. broadcasting refers to transmitting a packet that will be received by every device on the network
- can exist aso low as L2
	- meaning we can broadcast on ethernet
- broadcasting is not implemented on IPv6, since it is considered wasteful to broadcast a message to all nodes, when perhaps only a few need to know about it.
	- Instead, IPv6 uses multicast

### Multicast
group communication where data transmission is addressed to a group of destination computers simultaneously.
- can be 1:many or many:many distribution
- may exist on L7 (application) or L3 (network assisted).
	- if done on L3, the sender of the data can send it in a single transmission.
- multicasting limits the pool of receivers to those that join a specific multicast receiver group.

* * *
the combination of IP and port is an interesting thing. We have the IP address of a node (L3), and a TCP port that corresponds to a particular service running on that machine. This demonstrates how both are addresses for two different layers: L3 and L4. In other words, when the data is heading for the IP address, it is a packet. When it is heading for the port, it is a segment.


* * *
## UE Resources
[explanation of VLAN](https://serverfault.com/questions/188350/how-do-vlans-work)
