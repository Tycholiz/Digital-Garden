
Tailscale uses [WireGuard]([[network.vpn#wireguard-vs-openvpn]]) to create secure tunnels between nodes in a tailscale network.

To get into the network, a client must first authenticate with the Tailscale coordination server, which is essentially, a shared drop box for public keys.
- in reality, this coordination server handled authentication with 3rd party auth providers.

When a new node enters the tailnet, here's what happens:
1. The node generates a random public/private keypair for itself, and associates the public key with its identity (provided by 3rd party authentication service).
    - the private key is only ever known to the node that created it. As a result, only that node can encrypt packets addressed from itself, or decrypt packets addressed to itself, making them end-to-end encrypted (a concept called "zero trust networking").
2. The node contacts the coordination server and leaves its public key and a note about where that node can currently be found, and what domain it's in.
    - this public key then gets distributed to all other nodes in the tailnet.
3. The node downloads a list of public keys and addresses in its domain, which have been left on the coordination server by other nodes.
4. The node configures its WireGuard instance with the appropriate set of public keys.

In Tailscale, each node is responsible for blocking incoming connections that should not be allowed, at decryption time.

### Tailscale Funnel
Traditionally, in order to connect into a tailnet and be able to tunnel into the nodes that form it, the user would have to create a Tailscale account and be invited to the admin of the tailnet.

Using funnels gets us around this requirement, and users from the internet are able to access the tailnet.