
# Traceroute
- see a list of all nodes (routers and end node) that your packets traveled through on their way to an origin server. Each step is a *hop*
	- Also tells us how long each jump took (the response time b/w nodes) (RTT; round trip time)
- RTT tells us how long it took to get to that node (from the previous one) and return to your computer
	- There are three RTT columns because the traceroute sends three separate signal packets so that we may be able to spot inconsistencies in the route.
- The final column has the router IP
- `*` in the traceroute means that packets were lost
- consistency of RTTs between columns is what we are looking for when analyzing a traceroute.  
- when we use traceroute, the packet gets sent to the first router, which sends a packet back to the source. Then the packets continue on to the next router, which again sends a packet back to the source. This pattern continues until we reach the destination node.
	- ex. if there are 5 routers between a source node and destination node, then the source will send 6 packets into the network, which each packet addressed to the destination node.
		- The difference of course with traceroute over a regular packet, is that when an individual router along the chain receives the packet, it does not forward it along. Instead, it sends it back to the source. In this way, the source can determine the route that the packets took to reach the destination.

Issues:
- if the RTT from one hop to another greatly increases, and continues to increase until the destination, this indicates a problem with the node that first took a long time to respond. 
	- This often accompanies packet loss (`*`)
- if the RTT spikes on only one node, then subsequent hops have lower RTT, that doesn't indicate an issue. It just means the slow router gave your packets a lower priority.
- if the RTT jumps then remains consisistent at that levels, this does not indicate an issue
- By default, traceroute uses high UDP ports for tracing hosts. Sometimes firewalls block these UDP ports. 
	- use `-P` flag to use different protocols: (`-P ICMP`, `-P TCP`, `-P UDP`)
