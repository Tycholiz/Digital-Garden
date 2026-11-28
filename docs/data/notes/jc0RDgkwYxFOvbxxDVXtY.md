
## What is it?
A VPN connects your PC, smartphone, or tablet to another computer (called a server) somewhere on the internet, and allows you to browse the internet using that computer’s internet connection.
- So, instead of connecting to the internet directly via your ISP, you connect first to your VPN provider, which forwards your request on via the internet.
    - therefore, from the perspective of any node between the VPN and the origin server, the VPN is the client make the request.
- so is it basically a network with a dedicated server that executes the web searches, and delivers that content to the client
- the VPN is the thing on the server than enables the network to exist

A VPN is similar to a proxy server, but where a proxy server can only redirect web requests, a VPN connection is capable of routing and anonymizing all of your network traffic.
- also, a VPN works on the operating system level, meaning that it redirects all your traffic, whether it’s coming from your browser or a background app.

ex. If a company were to have 2 branches, they would not be able to communicate with one another with only their private IP addresses. They could choose to connect through the internet, but that would not be preferable, since traffic should be limited to those in the company. Instead, the company can use a VPN to bridge the two private networks. 
- alternatively, an IP tunnel can be used.
- with either method, the result is that we are encapsulating the packets within a protocol layer during trasmission across the public network.

## Why use one?
A VPN allows you to privately access online activities no matter where you are by encrypting your connection to the Internet

## Split VPN
You define some subnets (say 192.168.0.0/24) that should use the VPN, and the rest won't.
- ex. if we have a Plex server in our local network and we only want to expose access via VPN, we can opt to only route traffic over the VPN for the one subnet with the Plex server in it. Otherwise, users attempting to access the Plex server would have to toggle the VPN on/off each time they wanted to use it.

* * *

## Protocols
- ex. WireGuard, OpenVPN

WireGuard is the newest protocol which is 20-60% faster than OpenVPN, with state-of-the-art cryptography

### Wireguard vs OpenVPN
Traditionally, VPNs are connection servers that sit between a client and a server.

Because of the fact that the VPN server handles the work of making the connections between client and server, it is known as a type of *hub-and-spoke network*.
- a benefit of this type of network is that each node does not have to establish a connection with all of the other nodes 
    - (If you wanted to fully connect 10 nodes, then that would be 9 peer nodes that each node has to know about, or 90 separate tunnel endpoints.)
- The hub node usually has a static IP address and a hole poked in its firewall so it’s easy for everyone to find.

![](/assets/images/2024-03-26-21-11-33.png)

a critical downside of traditional VPNs is that they are at the mercy of the vpn server. If it is located far away, the client will have to incur a potentially large latency cost.

Unlike traditional VPNs, Wireguard is optimized to be able to create a large amount of encrypted tunnels. As a result, it's not subject to the limitations resulting from having a single centralized vpn.
- this ability to create lightweight encrypted tunnels makes Wireguard exceptional at establishing *mesh networks*, 
- however, this presents a new problem: how do we manage the distribution of keys to the vpn servers when a new user is authenticated? how about when a new vpn server gets added? How do we distribute its key to all of the users? Futhermore, a 10-node network would require 10 x 9 = 90 WireGuard tunnel endpoint configurations; every node would need to know its own key plus 9 more. Worse yet, each node would have to find the others somehow— a tough task given that user devices do not typically have static IP addresses

![](/assets/images/2024-03-26-21-30-21.png)
