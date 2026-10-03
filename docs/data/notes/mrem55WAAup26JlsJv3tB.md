
### Internet Daemon (inetd)
- To preserve system resources, UNIX handles many of its services through the internet daemon (inetd), as opposed to a constantly running daemon.
- inetd is a super server that listens to the various ports and handles connection requests as it receives them by initiating a new copy of the appropriate daemon (program). The new copy of the program then takes it from there and works with the client, and instead goes back to listening to the server ports waiting for new client requests to handle. Once the request is processed and the communication is over, the daemon exits.
