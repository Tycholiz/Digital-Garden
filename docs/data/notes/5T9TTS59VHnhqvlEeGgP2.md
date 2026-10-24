
A socket is the interface between the application layer and the transport layer, and can be thought of as the API between the application and the network

As application developers, we can think of the socket as the dividing point between what we have control over and what we *don't* have control over.
- realistically, the developer does have control over 2 things on the transport layer
    1. The choice of transport protocol
    2. transport-layer parameter, allowing control over things like maximum buffer size, maximum segment sizes etc.

It is not accurate to say that programs communicate with each other (eg. that an Express server program communicates with a Postgres server program). Realistically, the programs are running as processes on their respective hosts, and it is these processes that communicate with each other, via sockets.
- anal: If the processes were houses, then the sockets would be the doors that enable access to the processes

the socket's uniqueness is determined by five factors:
- the local IP address
- the local port number
- the remote IP address
- the remote port number
- the transfer protocol (TCP/UDP)

* * *

### How can more than 65,535 clients connect to a server?
While it's true that a machine can only open 65,535 ports, this does *NOT* mean that only 65,535 clients can connect to a server at a time. A server listens only on one port and can have large numbers of open sockets from clients connecting to that one port.
- On the TCP level the tuple (source ip, source port, destination ip, destination port) must be unique for each simultaneous connection. These 4 factors determine the socket's uniqueness.
    - That means a single client cannot open more than 65535 simultaneous connections to a single server. But a server can (theoretically) serve 65535 simultaneous connections per client.

In practice the server is only limited by how much CPU power, memory etc. it has to serve requests, not by the number of TCP connections to the server.
