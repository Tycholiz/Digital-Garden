
## Why use it?
[[IP|protocol.IP]] is unreliable, in that we can't guarantee that the data that's sent is received without being corrupted. IP cannot even guarantee that the data arrives. 
- TCP is reliable. Interestingly, TCP is built on top of IP. The question is, how is a reliable technology built on top of an unreliable one? The answer is that TCP acts as sort of the manager of a transaction of data from one place to another. If data arrives corrupted, it will repeat the request until data comes in fully-formed. It ensures that missing packets are retransmitted, duplicates are eliminated, and packets are reassembled into the order in which they were sent.
- Since the end-user never knows about the data-loss that might have occurred somewhere along the way, TCP is effectively an abstraction

TCP considers a packet to be lost if it is not acknowledged within some timeout (which is calculated from observed round-trip times), and lost packets are automatically retransmitted.
- Although the application does not see the packet loss and retransmission, it does see the resulting delay.

TCP is said to exist between the [[sockets|network.socket]] of two processes

On the TCP level, the tuple (source ip, source port, destination ip, destination port) must be unique for each simultaneous connection. That means a single client cannot open more than 65535 simultaneous connections to a single server. But a server can (theoretically) serve 65535 simultaneous connections per client.
- So in practice the server is only limited by how much CPU power, memory etc. it has to serve requests, not by the number of TCP connections to the server.

The connection between 2 processes via TCP is full-duplex, meaning the two processes can send messages to each other simultaneously.
- When the application finishes sending a message, it must tear down the connection

- TCP transports application-layer messages such as HTTP requests.
- Works by establishing a connection between 2 hosts.
- Data is guaranteed to be delivered, and in the order it was sent.
- TCP breaks long messages in to shorter segments.

### When not to use TCP
- TCP is not a suitable protocol for applications that require data to be transmitted in real-time or with strict timing requirements, such as audio or video streaming applications, where even minor delays or packet loss can lead to a degraded user experience. In cases like this a protocol like [[UDP|protocol.UDP]] is preferred.

### TCP flow control
TCP performs flow control (also known as congestion avoidance or backpressure), in which a node limits its own rate of sending in order to avoid overloading a network link or the receiving node. 
- This means additional queueing at the sender before the data even enters the network.

### Contrasting a TCP network connection with a Circuit-switched network (e.g. telephone connection)
A phone network is synchronous. 

When you make a call over the telephone network, it establishes a circuit: a fixed, guaranteed amount of bandwidth is allocated for the call, along the entire route between the two callers. This circuit remains in place until the call ends.
- an ISDN network runs at a fixed rate of 4,000 frames per second. When a call is established, it is allocated 16 bits of space per frame. Therefore, for the duration of the call, each side is guaranteed to be able to send exactly 16 bits of audio data every 250 microseconds.
- even as data passes through several routers, it does not suffer from queueing, because the 16 bits of space for the call have already been reserved in the next hop of the network
- also, because there is no queueing, the maximum end-to-end latency of the network is fixed. We call this a *bounded delay*.

This is very different from a TCP connection:
- a circuit is a fixed amount of reserved bandwidth which nobody else can use while the circuit is established.
- the packets of a TCP connection opportunistically use whatever network bandwidth is available.
