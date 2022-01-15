
- The OSI model is a framework for understanding how communications work in a computing system. It is an abstract representation, since no attention is paid to the implementation details of each layer. Instead, each layer simply describes its function and purpose. Put another way, it defines what input it expects, and what output it gives. 
	- Since each layer interfaces directly with the layers above and below it, the consistence of the input and output offered by each layer is the only thing that matters (ex. as long as L1 receives 0's and 1's and delivers frames, all other details are inconsequential)

## The Protocol Layers
### L7 Application	
- consists of network applications and their application-layer protocols
- primary user interface with communication system.
- PDU - messages
- HTTP, FTP, DNS, SMTP, POP3, SSH, IRC, TLS/SSL, NFS (network FS)
### L6 Presentation 
- Supports the functionality of the application layer by providing services such as formatting and translation of data.
- provide data encryption and data compression.
- Data representation (compression, decompression) and encryption
- ex. SSL, SSH, IMAP, FTP, TLS, MPEG, JPEG
### L5 Session
- Maintains the transmission path by synchronizing packets and controlling access to the medium by the Application layer.
- controls the connections between computers
- provides for delimiting and synchronizing of data exchange, including the means to build 
- ex. API, sockets, HTTP sessions
### L4 Transport
- Ensures the quality of transmission and determines the best route for transmission of data using the Network layer below.
- concerned with providing reliable communication over an unsecured network
- PDU - segments (TCP), datagrams (UDP)
- goal: deliver the data to the right software application
	- ex. TCP, UDP
### L3 Network 
- Finds a route for transmission of data (packets) between 2 routers/hosts, and establishes and maintains the connection between two connected nodes.
- goal: pass data chunks over multiple connected networks
- PDU - packet
- ex. IPv4, IPv6, ICMP, ARP, NAT
### L2 Data Link 
- Creates, transmits, and receives packets. Controls the Physical layer.
- Concerned with sharing multiple access channels
- They are used to deliver frames on a LAN
- PDU - frames
- goal: organize the 1s and 0s into chunks of data, and get them to the right place on the wire.
- ex. wifi, ethernet, bluetooth, VLAN, port forwarding procol
### L1 Physical 
- Converts data into bits for transmission and converts received bits into usable data for the layers above it.
- PDU - bits
- goal is to send 0s and 1s across a wire
- ex. fiber optic, copper wire, coaxial cable, wireless, modem, repeaters, ethernet (physical portion), USB

### The Internet Protocol
- The IP stack consists of L1, L2, L3, L4, L7
- When an HTTP request is sent, the protocol is established by piggybacking on the TCP connection that had already been made. This TCP connection is enabled by following the internet protocol. This is the point at which the internet protocol determines which routes datagrams
- The fact that there are 2 layers that are openly missing from the internet stack poses an interesting question: why are they not there? The reason is that the internet leaves these layers up to the application developer. The application developer can use any implementation of L5 and L6 that they choose in order to achieve their goals.

#### Routers
- As data is sent out, it starts at L7 and makes its way down to L1. At L1, it is connected to the link-layer switch and goes up to L2, before going back down to L1. Then it reaches the router, which goes up to L3, then back down to L1, to be repeated depending on the number of subsequent routers. At the final router, the the router's L1 communicates with the L1 of the destination host, as it makes its way back up to L7.

### Airplane trip analogy
- As we look at the process of planning and taking an airplane, it becomes apparent that there are different layers to the entire process. In fact, each layer appears two times in the whole process— in reverse order:
1. buy ticket
2. check baggage
3. load at gate
4. takeoff on runway
5. airplane routing (travel)
6. land of runway
7. unload at gate
8. pickup baggage
9. complain about ticket.

- each layer implements some functionality, and we can see that the opposite action was performed in reverse order.
- We can also see that each layer provides service to the layer below it.
	- ex. the act of checking baggage only makes sense to a ticketed person. 
	- ex. the idea of unloading at a gate only makes sense to a person on a landed plane.
- we notice that we can replace any layer in the model, as long as the functionality remains the same. In this way, layers are interfaces to each other, and don't care about each other's implementation— only the outcome (ie. output) it provides.

### Encapsulation
- as data travels each layer from L7 down to L1, additional information is added. As we pass the information encapsulated in the HTTP request down to L4, the transport layer takes the information of L7 and adds its own information to it. This information that was added is then used by the transport layer of the next node in the chain (likely the destination host). This process of encapsulation continues on down layer by layer until L1.
	- This idea of encapsulation thus demonstrates what is fundamentally different between each PDU: a message (L7) is an encapsulated datagram (L4). In other words, the datagram encapsulates the message. A datagram is fundamentally a message, plus some other information (provided by the layer). Therefore, at each layer, the PDU has 2 types of fields: header fields, and payload fields (the payload is just a packet from the layer above).

### Protocol Data Unit (PDU)
- each layer has the concept of a Protocol Data Unit, which is the format that the data exists in within the current Layer. In other words, it is what an atomic unit of data is called at each layer.
	- All PDUs are composed of a header and payload
	
### Miscelaneous
- the very fact that there are layers means that we can treat it as a modular chain, and swap out one L3 implementation for another (such as wifi for ethernet)
	- therefore L2 doesn't care if we are using IP or IPX on L3, just like L3 doesn't care if L2 uses wifi or ethernet
- The price we pay by layering is that we have to map between 32 bit IP addresses (L3) and 48 bit MAC addresses (L2).
	- The ARP protocol exists to solve this very problem
- The OS may be a participant in any or all of the layers
	- ex. at L1, signal processing can be offloaded to a host CPU and that requires a driver which interfaces with the operating system
