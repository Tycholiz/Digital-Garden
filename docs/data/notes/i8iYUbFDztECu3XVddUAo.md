
A registry is an organization responsible for maintaining a top-level zone’s datafiles, which contain the delegation to each subdomain of that top-level domain.

The DNS registry is a large database of all domain names and the associated registrant information in the TLDs of the DNS
	- With a database, third party entities (companies) can request administrative control of a domain name.
- Most registries operate on the top-level and second-level of the DNS.
- while registries manage domain names, they delegate the *reservation* of domain names to registrars

any given top-level domain can have no more than one registry

### Registrar
A registrar acts as an interface between customers and registries

Once a customer has chosen a subdomain of the top-level zone, the registrar submits to the appropriate registry the zone data necessary to delegate that subdomain to the nameservers the customer specified.

The registries act, more or less, as wholesalers of delegation in their top-level zone. The registrars then act as retailers, usually reselling delegation in more than one registry.

### Registration
Registration is the process by which a customer tells a registrar which nameservers to delegate a subdomain to and provides the registrar with contact and billing information.