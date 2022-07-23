
A *domain* is simply a subtree of the domain namespace.

A domain can be spread across multiple [[zones|dns.db.zone]]
- consider that those who manage the `com` domain don't manage the `facebook.com` domain; it has delegated responsibility to facebook.
    - `facebook.com` is within the `com` domain (and `facebook.com` domain), but is only in the `facebook.com` zone. It is not part of the `com` zone.

In the absense of subdomains (and thus delegation), the zone and the domain contain the same information

### Subdomain
The term *subdomain* is relative. The `facebook` domain is a subdomain of the `com` domain, and the `com` domain is a subdomain of the root domain.

The most common subdomain is `www`
- The www subdomain is so widely used that most domain registrars include it with domain name purchases.

The concept of subdomain can be achieved in 2 different ways:
1. The DNS zone file pertaining to the parent domain can be edited
    - in other words, subdomains are contained within parent domains (this is not the case for the second)
2. a record can be made to map a name to the [[A record|dns.records.a]].
    - network operations teams consider the second to not really be a subdomain, and instead restrict the definition to only including those subdomains which are provided by the zone NS records, and any server-destination other than that.

### Domain names
a domain name can be in many domains
- ex. `pa.ca.us` is part of the `ca.us` domain, but also part of the `us` domain.

![](/assets/images/2022-03-12-19-59-47.png)

Domain names are used as [[indexes|db.strategies.index]] into the [[DNS database|dns.db]].

Interior domain names (ie. not leaf nodes) can name a host and point to informa- tion about the domain; it doesn't have to be one or the other.
- ex. `hp.com` is both to Hewlett Packard's domain, as well as a domain name that refers to the host that runs HPs main webserver.

When you use a domain name, the information that is retrieved depends on the protocol we used to access that information
- ex. if we are using HTTP or [[unix.cli.ssh]], then we get the IP address. If we are using mail protocols, then we get mail-routing information.

### Apex domain
The apex domain is the root domain, without any `www` subdomain.

### Fully Qualified Domain Name (FQDN)
- The full domain name, rather than its parts separated. The FQDM is notable because it is fully unambiguous. It therefore points to a very specific place, and cannot be interpreted in any other way.
- It always ends in the TLD (therefore, paths don't count)
- FQDN in DNS records should always be appended with a `.`
	- This is to differentiate them from relative paths. Therefore, the trailing `.` serves the same purpose as the leading `/` when we're talking about a path.
	- Names without trailing dots are sometimes interpreted as relative to some domain name other than the root.
