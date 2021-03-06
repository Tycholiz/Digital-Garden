
DNS can be thought of as a directory service for the internet.
- Consider that a real-life directory service works like this: You have the name of someone, and you consult an agent to get "directions" (or a phone number) on how to reach them. A DNS is a similar entity which when given a domain name, can give us the address that it represents.

DNS illustrates how a piece of core network functionality (network-name to network-address translation) can be implemented at the application layer of the internet.

## History
DNS is based on a distributed database that takes time to update globally. In the early internet days, the database was small and could be updated by hand. As the internet grew, this became unfeasible, so a new management structure was introduced: the concept of *domain name registrars*. The idea was that the updates to the database would be handled by the registrars
- nowadays, when you make updates to domain name management settings, the registrar will push out the updated information to other DNS servers. 

The DNS can be thought of the administrative assistant of the internet. It carries all the administrative responsibility of making things work, albeit behind the scenes. 
The DNS is made up of Internet nameservers and a communication protocol

## Components of DNS
- [[DNS Cache|dns.cache]]
- [[DNS Lookup|dns.lookup]]
- [[DNS Resolver|dns.resolver]]

## Config
`/etc/resolv.conf`
- we can see what our system's DNS info is with this file
	- run `scutil --dns` on Mac to see DNS configuration

## Names
### Apex domain
The apex domain is the root domain, without any `www` subdomain.

### Fully Qualified Domain Name (FQDN)
- The full domain name, rather than its parts separated. The FQDM is notable because it is fully unambiguous. It therefore points to a very specific place, and cannot be interpreted in any other way.
- It always ends in the TLD (therefore, paths don't count)
- FQDN in DNS records should always be appended with a `.`
	- This is to differentiate them from paths.

* * *
There is no way to specify port numbers in DNS. If you are running a website, your server must respond to HTTP requests on port 80

# Resources
https://shapeshed.com/unix-traceroute/
https://activedirectorypro.com/dns-best-practices/#dns-aging-scavenging

# UE Resources
- [DNS sinkhole](https://en.wikipedia.org/wiki/DNS_sinkhole)
