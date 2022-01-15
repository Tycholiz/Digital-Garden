
When a DNS server queries other DNS servers, it’s making an “upstream” query. Queries for a domain can go “upstream” until they lead back to domain’s authority, or “authoritative name server.”
- An authoritative name server is where administrators manage server names and IP addresses for their domains. Whenever a DNS administrator wants to add, change or delete a server name or an IP address, they make a change on their authoritative DNS server (sometimes called a “master DNS server”).

Therefore, the authoritative nameserver is the server that actually holds, and is responsible for, DNS resource records
	- This could be considered the "domain's nameserver"
- Holds information specific to the actual domain name it serves (eg. google.com), and will send the IP address back to the recursive resolver.
	- The IP address it sends is found in the DNS A record.
- Each domain has its own authoritative NS
- spec: this nameserver holds the records that we see on our domain registrar dashboard for our domain. 
