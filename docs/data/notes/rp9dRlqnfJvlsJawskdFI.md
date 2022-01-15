
The most common subdomain is `www`
- The www subdomain is so widely used that most domain registrars include it with domain name purchases.

The concept of subdomain can be achieved in 2 different ways:
1. The DNS zone file pertaining to the parent domain can be edited
    - in other words, subdomains are contained within parent domains (this is not the case for the second)
2. a record can be made to map a name to the A record.
    - network operations teams consider the second to not really be a subdomain, and instead restrict the definition to only including those subdomains which are provided by the zone NS records, and any server-destination other than that.
