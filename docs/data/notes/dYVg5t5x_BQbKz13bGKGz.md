
UDP is a no-frills, lightweight transport protocol, providing minimal services.

UDP is connectionless, so there is no handshaking before the 2 processes start to communicate.

UDP provides an unreliable data transfer service, meaning that when a process sends a message into a UDP [[socket|network.sockets]], UDP provides no guarantee that the message will ever reach the receiving process. Furthermore, there is not even a guarantee that the messages will arrive in the order they were sent.
