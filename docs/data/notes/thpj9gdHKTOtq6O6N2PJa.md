
A Network Area Translation (NAT) allows us to map a router's public internet-facing IP address to 1+ local IP addresses on a LAN
- the NAT lives on the router.

the NAT was invented to solve a problem presented by the IPv4 protocol – a shortage of IP addresses
- when you are at a home network and access a webpage, the request goes from your local machine to the router, where the router translates your machine's IP address to the router's address. Therefore, the server that you connected to only sees your router's IP address as the origin of the request.

The NAT is the precise reason why if you search something on Google, the results don't show up on your dad's computer.

A side effect of NAT is that machines on the internet cannot initiate communications to local machines; they can only respond to communications initiated by them

NAT Table
- allows devices on a private network to access a public network
- each row in the table maps one private address to one public address.
- When the router receives an outbound request from a host in the LAN, it changes the request headers to have the public IP of the router, and it creates an entry in the NAT table.
	- When an external request comes in from the internet to the private network, the router needs to know where to forward those packets, so it looks in the NAT Table to find out which host to send it to.
- each pairing of our private host IP and external public IP is called a *connection*

## Hole Punching
- hole punching is a technique to establish a direct connection between two parties, where one or both are behind firewalls using NAT
- To punch a hole, each client connects to an unrestricted third-party server that temporarily stores private and public IP address and port information for each client. The server then relays each client's information to the other, and using that information, each client tries to establish direct connection;
	- once there is a successful connection using valid port numbers, each router accepts and forwards the incoming packets on to the host node on the LAN
- Hole punching is agnostic to which layer the hole punching is performed at. Therefore, there are different types of hole punching occurring at different layers:
	- ICMP hole punching
	- UDP hole punching
	- TCP hole punching
- Technologies that use hole punching include:
	- VoIP, online games, P2P networking, Skype

Why is this needed?
- networked devices with privately/publicly available IP addresses can connect easily. However, when one or both of the hosts are behind different firewalls, we need to implement hole punching to make the connection
