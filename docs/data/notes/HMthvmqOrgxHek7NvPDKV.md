
## What is the Internet?
The internet is a network of networks. 
- the first "network" in this phrase are the ISPs

The Internet is a collection of separate and distinct networks
	- The relationship between these networks is defined by one of the following:
		1. Transit (or pay) – The network operator pays money (or settlement) to another network for Internet access (or transit).
		2. Peer (or swap) – Two networks exchange traffic between their users freely, and for mutual benefit.
	- Therefore, in order for a network to reach any specific other network on the Internet, it must either:
		1. Sell transit (or Internet access) service to that network (making them a 'customer'),
		2. Peer directly with that network, or with a network which sells transit service to that network
- the internet is based on the principle that any Internet user can reach any other Internet user as though they were on the same network (*global reachability*)
	- Therefore, any Internet connected network must by definition either pay another network for transit, or peer with every other network which also does not purchase transit.
- Think of the internet as a huge tree with lots of leaves and branches. Your router is the stem of a single leaf (your home). It connects that leaf to the rest of the tree. It doesn't need to know where everything else is, it just needs to know how to transfer data between the twig it is on and the leaf it is connected to. The twig knows what it's connected to, and so on. Some parts of the tree connect directly to many other parts. For example, the trunk is connected to every branch. But, it still has to pass information through those branches and rely upon them to get it to the right twig and leaf.

The Internet is literally a network of networks, and it’s bound together by BGP. BGP allows one network (say Facebook) to advertise its presence to other networks that form the Internet. As we write Facebook is not advertising its presence, ISPs and other networks can’t find Facebook’s network and so it is unavailable.

The individual networks each have an ASN: an Autonomous System Number. An Autonomous System (AS) is an individual network with a unified internal routing policy. An AS can originate prefixes (say that they control a group of IP addresses), as well as transit prefixes (say they know how to reach specific groups of IP addresses).

Cloudflare's ASN is AS13335. Every ASN needs to announce its prefix routes to the Internet using BGP; otherwise, no one will know how to connect and where to find us.



### How data moves through a network of links and switches
There are two approaches to moving data through a network of links and switches: **circuit switching** and **packet switching**

#### Circuit Switching
- with *circuit-switched networks*, the resources needed along a path are reserved for the duration of the communication session. this is less efficient, since the circuit is still reserved even when no data is transmitted (ex. on phone call, the line is reserved even when no one is talking)

#### Packet Switching
- with *packet-switched networks*, the resources are not reserved, and the session's messages use the resources on demand, and therefore may have to wait for access to a communication link
	- here, resources would be buffers and the link transmission rate
- **Packet Switching** is the process of grouping data to be placed into a packet and sent over a network. 
- a **packet switches** comes in two types: routers and link-layer switches.
	
#### Examples
- anal: We have 2 restaurants: one that takes reservations and one that doesn't. The first has more initial setup, since we have to call to make the reservation— though there is less work to do once we arrive (circuit-switched). Meanwhile, the second takes less time to setup since we don't have to phone ahead, but we have to wait longer once we actually arrive at the restaurant (packet-switched).
	- In the non-reservable restaurant, the time spent waiting to get a table is analogous to a *queueing delay* that affects the packets. *Packet loss* would be analogous to arriving at the restaurant and encountering a big line and subsequently being asked to leave.
- ex: telephone networks are circuit-switched networks, since of you want to send data over a line, you must first establish a connection between sender and receiver. also, once a connection is made, a constant transmission rate is reserved for the duration of the connection
- ex: the internet on the other hand is a packet-switched network, since when data is sent over a network no bandwidth is reserved. if one of the links is congested because other packets need to be transmitted at the same time, and our packet will have to wait in a buffer at the sending side of the transmission link
