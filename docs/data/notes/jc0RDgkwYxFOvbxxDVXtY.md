
## What is it?
A VPN connects your PC, smartphone, or tablet to another computer (called a server) somewhere on the internet, and allows you to browse the internet using that computer’s internet connection
- So, instead of connecting to the internet via your ISP, you connect to it via your VPN
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