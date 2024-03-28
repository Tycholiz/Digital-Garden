
## Process
1. When `www.facebook.com` is searched, the browser DNS cache is checked to see if the domain and IP address key-value pair is stored, after that, the OS's cache, then the DNS server configured on the system (which might be the DNS server in the home router, the ISP (Internet Service Provider), or the public DNS server.)
2. after the browser cache is checked, the resolver intercepts the request and checks its own cache. 
3. If it doesn't have anything, it will make a request to the root server to see if it has it cached. 
	- the part of the URL corresponding to root server is the final `.` of `www.facebook.com.`
4. If it does not, then it proceeds down the chain until the authoritative nameserver. 
	- the next parts of the chain are `com`, then `facebook`, then `www`. each of these is essentially a zone

At any point, if the nameserver does indeed have the website cached, then it will return it to the resolver, who will proceed to return it to the client in domain form. 

## DNS Request
A DNS request is executed by the browser on a device. The first thing the OS checks is the hosts file. If the hosts file has an entry for the DNS, then this entry is always used, regardless of what comes next.
- If the hosts file turns up no result, then the network card settings will be queried. This can come from one of 2 places:
	1. IP addresses of DNS servers configured in router
	2. IP addresses of DNS servers configured on device itself (in which the DNS settings on router would be set to *manual*)
When the DNS resolves, the browser is enabled to connect to a web server or a CDN edge server 

DNS queries and responses are sent in plaintext (via UDP), which means they can be read by networks, ISPs, or anybody able to monitor transmissions (even when using HTTPS)

<!-- During a new DNS lookup, the lookup passes through the resolver, root server, and TLD server. -->

At each step of the DNS lookup, information is gathered and cached for later use
- Therefore, in a DNS lookup, the resolution process runs until either it reaches the DNS server (?) and gets the IP, or one of the stages returns a cached version of the website. 

DNS Lookup:
![DNS lookup](/assets/images/2021-08-01-21-31-10.png)

*Below may be roughly the same info as above, but it's a good resource. combine the 2 snippets when we have more time*:
1. A DNS request starts when you try to access a computer on the internet. For example, you type www.varonis.com in your browser address bar.
2. The first stop for the DNS request is the local DNS cache. As you access different computers, those IP addresses get stored in a local repository.  If you visited www.varonis.com before, you have the IP address in your cache.
3. If you don’t have the IP address in your local DNS cache, DNS will check with a recursive DNS server. Your IT team or Internet Service Provider (ISP) usually provides a recursive DNS server for this purpose.
4. The recursive DNS server has its own cache, and if it has the IP address, it will return it to you. If not, it will go ask another DNS server.
5. The next stop is the TLD name servers, in this case, the TLD name server for the .com addresses. These servers don’t have the IP address we need, but it can send the DNS request in the right direction.
6. What the TLD name servers do have is the location of the authoritative name server for the requested site. The authoritative name server responds with the IP address for www.varonis.com and the recursive DNS server stores it in the local DNS cache and returns the address to your computer.
7. Your local DNS service  gets the IP address and connects to www.varonis.com to download all the glorious content. DNS then records the IP address in local cache with a time-to-live (TTL) value. The TTL is the amount of time the local DNS record is valid, and after that time, DNS will go through the process again when you request Varonis.com the next time.

## Reverse DNS Lookup
- *Forward DNS lookup* is using an Internet domain name to find an IP address. *Reverse DNS lookup* is using an Internet IP address to find a domain name
	- when you put a URL in the address bar, the address is transmitted to a nearby router which does a forward DNS lookup in a routing table to locate the IP address
	- `PTR Record` is a [[RR|dns.records]] for enabling reverse DNS lookups, which is the exact opposite of an A record.
- Reverse lookups are commonly used by email servers, who check and see if an email message came from a valid server before bringing it onto their network.
	- Many email servers will reject messages from any server that does not support reverse lookups (the absense of a `PTR record` means reverse lookups aren't supported)

### The `in-addr.arpa` domain
When an IP address is resolved from a domain name, a subdomain of the `in-addr.arpa` domain is created.
- each subdomain value of this domain corresponds to the IP address of the resolved domain we searched for, though in reverse order
- ex. if the IP address of `blog.example.com` is `15.16.192.152`, then the corresponding node in the `in-addr.arpa` domain is `152.192.16.15.in-addr.arpa`, which maps back to our domain name `blog.example.com`
	- the `15.in-addr.arpa` zone contains the reverse-mapping information for all hosts whose IP addresses start with `15`.