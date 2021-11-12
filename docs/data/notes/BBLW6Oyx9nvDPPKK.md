
A Synology NAS can be configured to run a DNS server. Even once the server is set up, of course by default nothing will happen, since no one will be configured to ask the NAS for DNS records
- There are 2 ways we can achieve this: by host, or by the router
    - by host means we configure it on every single client on our network
    - 

### Host-based method
In Mac Network Preferences you can find a place to add DNS server IP addresses. You might have your ISP's DNS server IP, you may have Cloudflare's (`1.1.1.1` and `1.0.0.1`), you may have Google's (`8.8.8.8` and `8.4.4.8`)

You can replace one of these IP addresses with the IP of your NAS. It's important to retain one of the Cloudflare/Google IP addresses, so that if the NAS is down, the whole network isn't down.
- note: we want to configure our NAS DNS server so that it forwards requests on to the Cloudflare/Google DNS servers if the NAS doesn't have the IP address in its cache (rule: enable forwarders)

* * *

## Router DNS 
When our Mac has its DNS server configured as the router IP address, your router is running a caching DNS server, and setting itself as the DNS server via DHCP.
- Your router is acting as a DNS forwarder, you ask your router and your router asks a DNS server for you (the DNS server it forwards it to on your behalf can be configured in the Router settings)