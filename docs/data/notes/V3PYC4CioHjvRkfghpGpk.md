
Think of port forwarding as a secret tunnel that bores straight through your router’s firewall, allowing outside traffic to connect directly with a device inside your local network.

Port forwarding (or port mapping) allows remote computers to connect to a specific computer or service on a private network. This allows you to run a web server, game server or a service of your choosing from behind a router.
- In a typical network, the router has the public IP address and computers/servers obtain a private IP address from the router that is not addressable from outside the network. When you forward a specific port on your router, you are telling your router where to direct traffic for that port.
	- use the [canyouseeme utility](https://canyouseeme.org/) to verify the success of that process.
- Port Forwarding is the act of directly forwarding data packets from one interface (or physical link) to another. This is not really proxying; instead, think of railroad tracks: without port forwarding the track ends at a station, but with port forwarding the track seamlessly continues to the next station.
- If I were to try and ssh into my home computer from halfway around the world by using my home's public IP address, my home router would receive the request, and not know what to do with it, since the private IP address would then be needed to complete the send.
- We can specify to a router "hey, when you receive requests from the internet with ssh (port 22), I need you to pass them along to 192.168.1.74" 
	- We give the router a forwarding IP address (the internal IP that the packets are destined for) and a port. The router external IP and port number together serve as a unique identifier, and we specify what happens when the router receives the combination  
		- "upon receiving a request on port 8000, forward that request on to 196.168.1.74"
- we need to specify an **external port**, which is the port on the router that is open and facing the internet.
	- some numbers will be in use already, such as services from email and web server.
	- pick port above 5000
- Port forwarding settings are found in the router (using the modem's IP) 

### External port range
- normally identical to the internal port range. For security purposes, we may change this.
- We may not like the fact that people on the outside can see what ports are being used. 
	- ex. 5432 is the normal port for postgres. Recognizability like this introduces security issues.
- To get around this issue, we can employ external port ranges. We can specify 11111 as the external port, and 5432 as the internal port, and now whenever a request comes in with port 11111, it will get forwarded to the specified internal IP and the internal port. 

### Internal port
- port of the service running on the internal IP host. 
	- ex, we are running an express server on port 8000
- we may want to expose port 22 (SSH), port 21 (FTP) 

## Safety
- Port forwarding is generally considered safe as long as your network has a strong firewall. 
	- Port forwarding on Xbox and PlayStation is safe while port forwarding on PC or for camera surveillance comes with a little more risk. 

Tips:
- Only use safe, well-known ports
- Limit the number of ports you use
- Use port forwarding sparingly
- Adjust your firewall rules so only specific IP addresses can get access to the port
- Regularly update your router firmware and device operating systems
- Use a VPN on the devices you haven’t used with port forwarding to improve their security
- Monitor your network for suspicious activity

## Resources
- [Caddy Reverse Proxy: Setting up for home network](https://caddy.community/t/using-caddy-as-a-reverse-proxy-in-a-home-network/9427)
	- [[caddy]]
