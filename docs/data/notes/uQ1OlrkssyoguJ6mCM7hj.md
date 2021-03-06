
### DNS Resolver (aka. Recursive Resolver)
the recursive resolver is at the beginning of the DNS query (with the authoritative nameserver at the end).
- the resolver receives requests from web browsers, and in turn makes additional requests to the rest of the servers in a final effort to resolve the query. 
- a resolver will tell us what the IP is, given a domain name. By default, if a resolver cannot tell us, then that is it. However, the fact that it is recursive means that not only can we query a resolver, but a resolver can make queries itself. Put another way, if it doesn't have the answer we are looking for, it has the capacity to make queries itself. 
- After receiving a request for a webpage, the resolver will either respond with cached data (given to it by the nameservers), or will send a request to the *root nameserver*, followed by a request to a *TLD nameserver*, and then one last request to an *authoritative nameserver* (This is called [[DNS Lookup|dns.lookup]])
	- At each step, there is a possibility that a cached version of the website will be found, and it can immediately be returned to the client, rather than continuing to recurse down the DNS tree.
- After receiving the requested IP address (from the authoritative nameserver), the recursive resolver sends a response to the client

DNS Client (aka *Resolver*) is a client machine configured to send name resolution queries to a DNS server
- in other words, our computer is a DNS client when it sends requests to port 53 of a DNS server, with the intention of resolving an IP address  
