
The DNS nameservers constitute the server half of the system's [[client-server architecture|general.arch.client-server]]
- [[Resolvers|dns.resolver]] are the client

Nameservers contain information about some segments of the [[DNS database|dns.db.zone]] and make that information available to [[resolvers|dns.resolver]].
- nameservers have complete information about some part of the domain namespace. This information is either loaded directly from a file (if the nameserver is authoritative about that particular zone), or from another (upstream) nameserver. This is called *name resolution*.
- if the nameserver has the information on file, then it is known as [[authoritative|dns.nameserver.authoritative]]

A nameserver needs only one piece of information to find its way to any point in the tree: the domain names and addresses of the root nameservers. 
- A nameserver can issue a query to a [[root nameserver|dns.nameserver.root]] for any domain name in the domain namespace, and the root nameserver will start the nameserver on its way.

Each nameserver queried either gives the querier information about how to get closer to the answer it’s seeking or provides the answer itself.

Nameservers listen on port 53

- A DNS nameserver is a server that stores the DNS records for a domain
	- a DNS nameserver responds with answers to queries against its database.
- Nameservers point to DNS providers
- A nameserver is a computer designated to translate domain names into IP addresses
- Nameservers can be "authoritative", meaning that they give answers to queries about domains under their control. Otherwise, they may point to other servers, or serve cached copies of other name servers’ data.
- nameservers make your zone’s data available to all the other nameservers on the network.
- due to the distributed nature of the DNS database, nameservers are able to navigate through the database and find data in any zone.
- more than one nameserver can store data about a zone, which sometimes leads to inconsistencies between copies of the zone data.

## Master and Slave Nameservers
The DNS specification defines two types of nameserver: Master and Slave (a.k.a primary master and secondary master)
- A master nameserver for a zone reads the data for the zone from a file on its host
- A slave nameserver for a zone gets the zone data from another nameserver authoritative for the zone, called its master server
	- often, the master server is the zone's master

A slave can load zone data from another slave.

When a slave starts up, it contacts its master nameserver and, if necessary, pulls the zone data over (*zone transfer*).

Both the master and slave nameservers are [[authoritative|dns.nameserver.authoritative]] for that zone.

Once you’ve created the data for your zone and set up a master nameserver, you don’t need to copy that data from host to host to create new nameservers for the zone. All you need to do is set up slave nameservers that load their data from the primary master for the zone.
- when new zone data becomes available, the slaves will handle its transfer.

a nameserver can be a master for one zone and a slave for another.

master nameservers load their zone data from *zone datafiles*
- these datafiles contain [[resource records|dns.records]] that describe the zone.

## DNS Server
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
