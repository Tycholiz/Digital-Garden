
The `A` record is the most fundamental record
maps the IP address to a given domain
- Therefore, the `A record` serves as a lookup for the IP address of a given domain
- it is normal to have just one A record. 
- `AAAA` for IPv6

a value of 14400 for TTL means that if an A record gets updated, it takes 240 minutes (14400 seconds) to take effect.

Most websites have a single A record, but with multiple A records, you can implement round robin load balancing
