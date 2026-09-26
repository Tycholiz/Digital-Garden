
IP (the Internet Protocol) is unreliable: it may drop, delay, duplicate, or reorder packets.

## IP Address
IP addresses consist of 32 bits, which is why the address is broken down into 4 parts of 8 bits each.
- 255 represents 8 bits.

IP addresses are hierarchical, just like [[domain names|dns.domain]].
- subnetting is the same concept as creating multiple different subdomains. The only difference is that while domains get more specific from left to right, IP addresses get more specific from right to left.

### IP Address Spaces
- there are 2 main IP Address Spaces: public and private.
	- public are routable on the internet, meaning every device on the internet needs its own pubic IP
- the public address space is further divided into 5 classes:
Class A	0.0.0.0 – 126.255.255.255
Class B	128.0.0.0 – 191.255.255.255
Class C	192.0.0.0 – 223.255.255.255
Class D	224.0.0.0 – 239.255.255.255
Class E	240.0.0.0 – 255.255.255.255
	- A,B,C - devices directly connected to internet
		- ex. L3 switches, routers, firewalls, servers
	- D - multicast traffic
	- E - experimental
- The private address space is divided into 3 classes:
Class A—10.0.0.0/8 network block	10.0.0.0 – 010.255.255.255
Class B—172.16.0.0/12 network block	172.16.0.0 – 172.31.255.255
Class C—192.168.0.0/16 network block	192.168.0.0 – 192.168.255.255

### Address block
![[network.lan.router#seeing-all-nodes-on-a-local-area-network,1]]

### CIDR (Classless Inter-Domain Routing)
CIDR is a method for allocating IP addresses and for IP routing.
- it also allows for a flexible and simplified way to identify IP addresses and route network traffic.

It is similar to how telephones use:
- An area code to specify a geographical region
- A number to identify a specific device

CIDR notation consists of
- an IP address
- a forward slash (`/`) 
- a number that ranges from 0 to 32
	- represent a range of IP addresses
		- Every time the number decreases by one (starting at 32), it means the number of IP addresses in that range are doubled.

IP addresses are described as consisting of two groups of bits in the address
1. the *network prefix*, which are the most significant bits are the network prefix, and which identify a whole network or subnet
2. the *host identifier, which specifies a particular interface of a host on that network.