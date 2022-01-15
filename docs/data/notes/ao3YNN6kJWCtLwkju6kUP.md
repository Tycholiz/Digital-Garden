
### NS 
- nameserver record - indicate which authoritative nameserver contains the actual DNS records
	- Basically, NS records tell the Internet where to go to find out a domain's IP address
- A domain often has multiple NS records which can indicate primary and backup nameservers for that domain. 
	- if one nameserver goes down or is unavailable, DNS queries can go to another one
	- Typically there is one primary nameserver and several secondary nameservers, which store exact copies of the DNS records in the primary server
		- Updating the primary nameserver will trigger an update of the secondary nameservers as well.
- Without properly configured NS records, users will be unable to load a website or application.
- When multiple nameservers are used (as in most cases), NS records should list more than one server
- NS records must point to an A record
- ex. the resolver may have the NS records, but no A record, and it will still be able to query those nameservers directly, rather than having to go through the TLD server

Updating NS records
- Domain administrators should update their NS records when they need to change their domain's nameservers
- update NS records if you want a subdomain to use different nameservers than the domain (ex. example.com and blog.example.com have 2 different nameservers)
