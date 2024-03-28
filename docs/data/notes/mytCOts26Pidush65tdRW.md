
## What is it?
DNS can be thought of as a directory service for the internet.
- Consider that a real-life directory service works like this: You have the name of someone, and you consult an agent to get "directions" (or a phone number) on how to reach them. A DNS is a similar entity which when given a domain name, can give us the IP address that it represents.
- DNS is used by virtually all inter-networking software, including [[email|email]], [[ssh|unix.cli.ssh]], [[FTP programs|protocol.FTP]], and [[browsers|browser]]
- DNS is essentially made up of geo-replicated caches

The DNS is a [[distributed database|db.distributed]]. The management of host information is split among many sites and organizations
- You don’t need to submit your data to some central site or periodically retrieve copies of the "master" database. You simply make sure your section (called a [[zone|dns.db.zone]]) is up to date on your [[nameservers|dns.nameserver]].
- This structure allows local control of the segments of the overall database, yet data in each segment is available across the entire network through a client/server scheme.
- [[replication|db.distributed.replication]] and [[caching|deploy.distributed.cache]] is used to achieve robustness and high performance.

The DNS can be thought of the administrative assistant of the internet. It carries all the administrative responsibility of making things work, albeit behind the scenes. 

The DNS is made up of Internet nameservers and a communication protocol

note: There is no way to specify port numbers in DNS. If you are running a website, your server must respond to HTTP requests on port 80

## why use it?
If it were just us, we could keep a simple key-value storage on our computer of IP addresses and domain names. 
- The DNS system takes this simple concept and makes it remotely accessibly to anyone on the internet.
- in reality, more than just the IP is stored in the [[dns.db]]. Also included is information about mail routing, etc.

DNS illustrates how a piece of core network functionality (network-name to network-address translation) can be implemented at the application layer of the internet.

## History
DNS is based on a distributed database that takes time to update globally. In the early internet days, the database (a simple `HOSTS.txt` file) was small and could be updated by hand. As the internet grew, this became unfeasible, so a new management structure was introduced: the concept of *domain name registrars*. The idea was that the updates to the database would be handled by the registrars
- nowadays, when you make updates to domain name management settings, the registrar will push out the updated information to other DNS servers. 

# Resources
https://shapeshed.com/unix-traceroute/
https://activedirectorypro.com/dns-best-practices/#dns-aging-scavenging

# UE Resources
- [DNS sinkhole](https://en.wikipedia.org/wiki/DNS_sinkhole)
- [How DNS works guide](https://howdns.works/)
