
# Internet Protocol
IP addresses consist of 32 bits, which is why the address is broken down into 4 parts of 8 bits each.
- 255 represents 8 bits.

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
![[dendron://code/network.lan.router#seeing-all-nodes-on-a-local-area-network,1]]
