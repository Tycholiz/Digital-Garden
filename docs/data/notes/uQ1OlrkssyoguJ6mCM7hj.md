
The DNS resolvers constitute the client half of the system's [[client-server architecture|general.arch.client-server]]
- [[dns.nameserver]] are the server

Resolvers are often just functions that create name resolution queries and send them across a network to a [[nameserver|dns.nameserver]]. When the response is received, it hands it back to the program that requested it in the first place (e.g. spec: a browser)
- in other words, our computer initiates the resolver to query a nameserver at port 53, with the intention of resolving an IP address  

The DNS resolver (ie. recursive resolver) is at the beginning of the DNS query (with the authoritative nameserver at the end).
- the resolver receives requests from web browsers, and in turn makes additional requests to the rest of the servers in a final effort to resolve the query. 
- a resolver will tell us what the IP is, given a domain name. By default, if a resolver cannot tell us, then that is it. However, the fact that it is recursive means that not only can we query a resolver, but a resolver can make queries itself. Put another way, if it doesn't have the answer we are looking for, it has the capacity to make queries itself. 
- After receiving a request for a webpage, the resolver will either respond with cached data (given to it by the nameservers), or will send a request to the *root nameserver*, followed by a request to a *TLD nameserver*, and then one last request to an *authoritative nameserver* (This is called [[DNS Lookup|dns.lookup]])
	- At each step, there is a possibility that a cached version of the website will be found, and it can immediately be returned to the client, rather than continuing to recurse down the DNS tree.
- After receiving the requested IP address (from the authoritative nameserver), the recursive resolver sends a response to the client

## Configuration file
The resolver is configured in `/etc/resolv.conf`
- run `scutil --dns` on Mac to see DNS configuration

### Directives
#### `search`
For example, the directive:
```
search corp.hp.com paloalto.hp.com hp.com
```
instructs the resolver to search the corp.hp.com domain first, then paloalto.hp.com, and then the parent of both domains, hp.com.

#### `nameserver`
tells the resolver the IP address of a nameserver to query. For example, the line:
```
nameserver 15.32.17.2
```
instructs the resolver to send queries to the nameserver running at the IP address 15.32.17.2 instead of to the local host. This means that on hosts not running nameservers, you can use the nameserver directive to point them at a remote nameserver. Typically, you configure the resolvers on your hosts to query your own nameservers.