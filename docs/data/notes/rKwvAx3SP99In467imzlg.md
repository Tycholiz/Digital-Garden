
A DNS Zone is an autonomously administered piece of the [[namespace|dns.db]]

The zone contains pointers to the [[authoritative nameservers|dns.nameserver.authoritative]] (ie. their IP addresses).

A new zone is created when a TLD (e.g. `.com`) delegates responsibility of a domain to the new owner (e.g. `facebook.com`)
- a new zone may or may not be created when subdomains are made (e.g. `developer.facebook.com`), depending on if `facebook.com` administrators delegate responsibility for it to other organizations. 

Consider that there is a hierarchy in who manages each domain. The root zone is managed by ICANN. The `edu` domain is managed by EDUCAUSE, but delegates responsibility of the `berkley.edu` domain to U.C. Berkeley. 
- when the authority for `berkley.edu` is delegated, a new zone is created. The new `berkley.edu` zone is now independent from `edu` and contains all domain names that end in `berkeley.edu`. Also, the `edu` zone no longer contains `berkley.edu`, since that domain now exists in a delegated zone.
- anal: consider that when we [[remotely mount a filesystem|fs.network]] our own, that directory structure looks like it is hosted on our machine. If I mount a [[NAS|nas]] to my local filesystem, the NAS is still responsible for the filesystem at `/Volumes/Synology-NAS`.
  - therefore, each zone can be thought of as a mounted volume to the higher level zone (ie. the `berkley.edu` zone is mounted on the `edu` zone, the `edu` zone is mounted on the root zone).

A zone contains all the domain names of the domain, unless it has been delegated.
- ex. the `ca` domain has subdomains `bc.ca`, `ab.ca`, `sk.ca`. The authority for each one *may* be delegated to [[nameservers|dns.nameserver]] in each province.
	- once authority has been delegated, the `ca` domain is only left with pointers to the subdomains.

This diagram shows how it may be the case that the `ca` domain has delegated authority of `ab`, `on` and `qc` to the respective provinces, but continues to manage the `bc` and `sk` domains (perhaps because provincial authorities in those provinces aren't yet ready to manage their own subdomains.)
- this shows that the [[zone|dns.db.zone]] is what the nameserver loads; not the [[domain|dns.domain]]
![](/assets/images/2022-03-12-20-49-28.png)

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
