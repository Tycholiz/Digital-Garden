
Domains can be very large, so they are further organized into smaller books, called, “zones.”
- The DNS is broken up into different zones. A DNS zone is a portion of the DNS namespace that is managed by a specific organization or administrator
- A DNS zone is a subset, often a single domain, of the hierarchical domain name structure of the DNS
	- if DNS was a filesystem, a DNS zone would be each directory.
	- ex. if we are talking TLD DNS zone, then examples are `.com`, `.net`. If we are talking Domain-level DNS, then examples are `facebook.com`, `google.com`
- A portion of the domain name space where administrative responsibility has been delegated to a single manager
- The DNS Zone is described by the Zone File (aka. DNS record), which serves as the database for each nameserver.
	- The zone file contains mappings between domain names and IP addresses, along with other resources. This file is organized around resource records (A, CNAME etc.). In other words, resource records form the basis of the database. 
	- We recognize the zone file when we go to a domain registrar, click onto one of our domains, and see all of the RRs that we have made.
- a 'zone' is an area of control over namespace. A zone can include a single domain name, one domain and many subdomains, or many domain names. 
	- In some cases, 'zone' is essentially equivalent with 'domain,' but this is not always true.
	- each zone has a *zone serial number*, which is a unique identifier
		- A DNS server can quickly look up a zone's records in its database via the serial number, which will bring up the SOA record.
- A common mistake is to associate a DNS zone with a domain name or a single DNS server
	- In fact, a DNS zone can contain multiple subdomains and multiple zones can exist on the same server
- We can decide which URLs should be their own zone, and which should be combined into a single zone.
	- ex. Below, as far as Cloudflare subdomains go, we have `blog`, `support`, and `community`. Support and community are small, so we put them in the same zone as the main `cloudflare.com`. However, the `blog` subdomain is a robust independent site that needs separate administration, so we give it its own zone.

Each below is an example of a zone:
![](/assets/images/2021-03-07-15-16-30.png)
![](/assets/images/2021-03-07-15-16-44.png)

### Zone Apex
- Where `SOA` and `NS` records live. They are records whose names are the same as the zone itself.
