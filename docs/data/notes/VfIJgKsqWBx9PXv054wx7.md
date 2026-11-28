
The root nameservers know where the [[authoritative nameservers|dns.nameserver.authoritative]] for each of the top-level [[zones|dns.db.zone]] are. 

There are 13 sets of root nameservers strategically distributed across the globe, each managed by different organizations. 
- These servers are responsible for providing the starting point in the DNS resolution process, which is kicked off when the [[resolver|dns.resolver]] queries the root nameserver for the given TLD.
- When queried, the root nameserver accepts the domain name and returns the names and addresses of the nameservers that are authoritative for the top-level zone the domain name ends in (e.g. `com`)
- The word "root" in the term is with reference to a root in a filesystem. It is the set that contains all TLDs. This is why we call them *root nameservers*. They live at the root, and provide preliminary information about specific TLDs.


note: while there are only 13 sets of root nameservers, they are replicated and distributed across multiple physical locations worldwide to ensure redundancy and resilience of the DNS system.
- ex. **F-Root Server** -->
