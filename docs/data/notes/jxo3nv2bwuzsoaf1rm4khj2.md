
a firewall is a network security system that monitors and controls incoming and outgoing network traffic based on predetermined security rules.
- the purpose is to establish a barrier between a trusted and an untrusted network
- the rule might be "block all requests originating from IPs in India", or "block all requests to ports 10000-20000"

Firewalls are either network-based (e.g. [[LAN|network.lan]]) or host-based
- Network-based firewalls are positioned between two or more networks (e.g. between LAN and WAN)
- Host-based firewalls are deployed directly on the host itself to control network traffic or other computing resources.

firewall rules are usually based on IP addresses, not users or roles, so they can be very awkward to configure safely. As a result, you end up having to add more layers of authentication, at the transport or application layers (e.g. HTTPS).