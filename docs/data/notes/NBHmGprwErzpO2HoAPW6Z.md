
### CNAME 
Canonical name record
- points to a domain, not an IP address
- CNAME records allow a machine to be known by more than one hostname
- map an alias name to a true (canonical) domain name
- typically used to map a subdomain, like `www.stuff.com` to `stuff.com`, or `stuff.com` to `blog.stuff.com`)
	- This is good, because if the host IP address changes, then we only need to change the A record. The CNAME record depends on the domain, not the IP address it's associated with.
- anal. Imagine a scavenger hunt where each clue points to another clue, and the final clue points to the treasure. A domain with a CNAME record is like a clue that can point you to another clue (another domain with a CNAME record) or to the treasure (a domain with an A record).
- ex. imagine we give `blog.tycholiz.com` a CNAME with value `tycholiz.com`. This means that any time a DNS server hits the DNS records for `blog.tycholiz.com`, it actually triggers another DNS lookup to `tycholiz.com`, since we specified that as the CNAME
	- in this example, the canonical name (true name) is `tycholiz.com`
	- from this example, you can see how CNAMEs are kind of like relays, since they don't map to an IP at all, but point to a domain name, which maps to an IP. In other words, CNAME records cause A records to resolve domain names. 
- The CNAME record only points the client to the same IP address as the root domain
	- Therefore, the CNAME record does not have to resolve to the same website as the domain it points to.
	- ex. in the case where we hit `blog.example.com`, the DNS will return us the same IP as if we hit `example.com`.  
		- when the client actually connects to that IP address, the web server will look at the URL, see that it is blog.example.com, and deliver the blog page rather than the home page.
- Pointing a CNAME to another CNAME is possible, but there is no point

 TEST
