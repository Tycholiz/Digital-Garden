
Routers are unique in that they have 2 IP addresses: a public WAN-facing one, and a private LAN-facing one.
- Routers perform the "traffic directing" functions on the Internet, forwarding packets from one network to another
	- in other words, data packets are forwarded through the networks of the internet from router to router until they reach their destination computer (with routing algorithms determining the choice of route.)
	- A router is like a railroad junction: from one incoming track, there are multiple possible destinations, so which one does it choose? You have to configure that (configurable through [[port forwarding|network.lan.router.port-forwarding]])
- Each router has a prior knowledge only of networks attached to it directly
- routers gain knowledge of the topology of the network when the routing protocol shares the information of who the router's neighbors are. Each neighbor then shares this information with *their* neighbors, and so on until the whole network is revealed. 
- routing protocols are layer management protocols for L3, regardless of their transport mechanism
	- in other words, data may very well travel over L2 or L4
- **DSL router** - a residential-grade router designed to create LANs and connect them to a WAN (which is provided by the ISP)
	- aka residential gateway
- a residential router uses a modem to connect the LAN to the WAN

### How routers determine which route to take
- Each packet contains an address in its header, which has a hierarchical structure (like a postal code)
	- Each router has a forwarding table which is used to compare against a part of the address to determine the next step in the packet's journey
	- Specifically, the routing table is like a hashmap that makes portions of the destination address to outbound links.
- each time a packet arrives at a router, the router consults its routing table.
	- This routing table contains the network ID and the host ID

### Routing Table
Whenever a node needs to send data to another node on a network, it must first know where to send it. If a direct connection can't be made, then the data has to be sent via other nodes along a route to the destination node.
- Imagine a node somewhere along this chain receives a packet of data. It has no idea where it came from or where it's going. A routing table solves this problem, as it gives each node in the chain the address for the destination node. 
	- Effectively, the router says "I don't know how to deal with 192.168.0.34, but I know that 192.168.0.254 (a router) knows, so if I get a packet destined for that address, I'll just pass it along to that router, since he knows how to deal with it." 
- A routing table is a database that keeps track of paths and uses these to determine which way to forward traffic.
- A routing table is a data file in RAM that is used to store route information about directly connected and remote networks.

* * *

### Core routers
- core routers are the supercomputers of the internet
- designed to operate on the internet backbone, as opposed to on the edge of a network (edge router, ex. home network).
- the core router's purpose is to forward ip packets along. 
- edge routers connect to core routers

* * *

### Seeing all nodes on a local area network:
We can see all nodes on a local area network with:
- `sudo nmap -sn 192.168.1.0/24`
	- the 24 is CIDR notation, signifying that we will scan from 192.168.1.0 to 192.168.1.255
		- the inclusion of 24 means we are scanning an *address block* ^RUsJkF0C
