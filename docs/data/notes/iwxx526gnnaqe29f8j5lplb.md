
we can use *nslookup* to send queries in much the same way that [[resolvers|dns.resolver]] do.
- we can also use it to query other nameservers as a nameserver would

The biggest difference between *nslookup*’s behavior and the resolver’s behavior is that *nslookup* talks to only one nameserver at a time.
- in `/etc/resolv.conf`, if there are 2 nameservers listed, then the resolver will try the first, then the second. On the other hand, `nslookup` will only ever try the first.

By default, nslookup looks up [[A records|dns.records.a]]
- if you type in an IP address (and the nslookup query type is A or PTR), nslookup inverts the address, appends in-addr.arpa, and looks up PTR records instead

We can also use `https://dnslookup.online`