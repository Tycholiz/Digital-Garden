
Port Knocking is a technique to externally open a port on a firewall.
- This is done by externally generating a connection attempt on a set of prespecified closed ports. Once the correct sequence of connection attempts is received, the firewall rules are dynamically relaxed to allow the external host to connect over the specified port. 
	- this is similar to a secret handshake
- to implement port knocking, we implement a daemon that watches the firewall log for connection attempts. If the attempted sequence is correct, then the daemon sends instructions to modify the firewall rules for the external host.
	- daemon examples: *knockd*
- anal. Imagine we had a 9 pane glass window in a house, and we have a friend on the inside, while we remain on the outside. In private, my friend and I agree that if I tap each pane in a sequence, then he will open up the 3rd pane. For instance, let's imagine the code is: 1, 5, 9, 2, 2, 3. If I don't tap the panels in order, then my friend will simply ignore me. If I am correct however, he will open the 3rd panel
	- here, each panel represents a port, I represent an external host, and my friend represents the router/firewall of the network I am trying to access.
