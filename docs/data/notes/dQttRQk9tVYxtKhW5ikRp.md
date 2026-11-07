
UDP is a no-frills, lightweight transport protocol, providing minimal services.

UDP is connectionless, so there is no handshaking before the 2 processes start to communicate.

UDP is a fire-and-forget protocol

UDP is a disconnected protocol in which the recipient of a datagram doesn’t send any acknowledgment to the sender

UDP provides low-overhead and faster transmission, but with less reliability and ordered delivery.

UDP provides an unreliable data transfer service, meaning that when a process sends a message into a UDP [[socket|network.sockets]], UDP provides no guarantee that the message will ever reach the receiving process. Furthermore, there is not even a guarantee that the messages will arrive in the order they were sent.

UDP does not perform flow control and does not retransmit lost packets like [[TCP|protocol.TCP]]

UDP is a good choice in situations where delayed data is worthless. 
- ex. in a VoIP phone call, there probably isn’t enough time to retransmit a lost packet before its data is due to be played over the speakers. In this case, there’s no point in retransmitting the packet.