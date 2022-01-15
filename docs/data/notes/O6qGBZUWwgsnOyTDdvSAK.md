
# DNS Request
A DNS request is executed by the browser on a device. The first thing the OS checks is the hosts file. If the hosts file has an entry for the DNS, then this entry is always used, regardless of what comes next.
- If the hosts file turns up no result, then the network card settings will be queried. This can come from one of 2 places:
	1. IP addresses of DNS servers configured in router
	2. IP addresses of DNS servers configured on device itself (in which the DNS settings on router would be set to *manual*)
When the DNS resolves, the browser is enabled to connect to a web server or a CDN edge server 

DNS queries and responses are sent in plaintext (via UDP), which means they can be read by networks, ISPs, or anybody able to monitor transmissions (even when using HTTPS)

# Nameservers
- A DNS nameserver is a server that stores the DNS records for a domain
	- a DNS nameserver responds with answers to queries against its database.
- A nameserver is a computer designated to translate domain names into IP addresses
- Nameservers can be “authoritative”, meaning that they give answers to queries about domains under their control. Otherwise, they may point to other servers, or serve cached copies of other name servers’ data.

# DNS Server
There are only 4 types of DNS Server:
1. DNS Recursor
	- the server that responds to a DNS query and asks another DNS server for the address, or already has the IP address for the site saved.
2. Root nameserver
	- A root name server is the name server for the root zone. It responds to direct requests and can return a list of authoritative name servers for the corresponding top-level domain.
3. TLD nameserver
	- The top-level domain server (TLD) is one of the high-level DNS servers on the internet. When you search for www.varonis.com, a TLD server for the ‘.com’ will respond first, then DNS will search for ‘varonis.’
4. Authoritative nameserver
	- The authoritative name server is the final stop for a DNS query. The authoritative name server has the DNS record for the request.

- In instances where the query is for a subdomain such as `foo.example.com`, an additional nameserver will be added to the sequence after the authoritative nameserver, which is responsible for storing the subdomain’s `CNAME` record.
- the ISP typically supplies the nameserver, but you can use public servers, like those offered by Google (which have IP `8.8.8.8` and `8.8.4.4`) or Cloudflare (`1.1.1.1` and `1.0.0.1`)
	- You could consider each IP address here to be a resolver.
- The DNS server has expanded its role beyond only resolving domain names, and has other anciliary functionality
	- ex. a real-time blackhole list for spam
- The DNS database is traditionally stored in a structured text file (the *Zone File*)
	- The Zone File describes a DNS Zone, and contains all RR for every domain in the zone.

Your home network typically relies on a DNS Server supplied by your ISP
- therefore your ISP's DNS servers see every domain you request.
- some ISPs have found a way to monetize their DNS service. When you hit an erroneous domain, one that has no actual IP address, they divert your browser to a search and advertising page preloaded with a search phrase derived from the domain name

### Library Analogy
- *Resolver* - a librarian who, given a title, is asked to go fetch a book
- *Root NS* - the blocks of bookshelves in the library
- *TLD NS* - the specific rack within the bookshelf block
- *Authoritative NS* - the specific book you asked for.

## Misc
the same domain name may have multiple IP addresses associated with it.

Anything that can be done with a DNS address can also be done with an IP address, since all a DNS does is translate from hostname (www.____.com) to IP.
