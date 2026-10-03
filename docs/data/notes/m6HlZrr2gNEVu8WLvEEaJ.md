
There are multiple caches involved in the entire DNS. They are checked in sequential order. 
1. Browser
2. OS - the process that handles this query is called a **stub resolver**, or **DNS Client** 
	- the stub resolver will first check to see if it has the cached data, and if not, will call the DNS query that gets handled by the resolver (which is hosted by the ISP).

A residential router internally runs a DNS cache and DNS proxy server
- it also advertises itself as the DNS server in all [[DHCP|network.lan#Dynamic-Host-Configuration-Protocol-(DHCP)]] responses.

There are actually DNS caches at every hierarchy of the lookup process
- The computer reaches your router, which contacts your ISP, which might hit another ISP before ending up at what's called the "root DNS servers." Each of those points in the process has a DNS cache for the same reason, which is to speed up the name resolution process.
```js
const cachedData = {
    "example.com": {
        "A": {
            "value": "192.0.2.1",
            "TTL": 3600 // Time-to-Live in seconds
        }
		"CNAME": {
            "value": "info.example.com",
            "TTL": 3600 // Time-to-Live in seconds
        }
    },
    "nonexistent.example.com": {
        "NXDOMAIN": {
            "TTL": 300 // Time-to-Live for negative response
        }
    },
    "authority.example.com": {
        "NS": {
            "value": ["ns1.example.com", "ns2.example.com"],
            "TTL": 86400 // Time-to-Live for authority information
        }
    },
    "root_servers": {
        "value": ["198.41.0.4", "192.228.79.201"],
        "TTL": 86400 // Time-to-Live for root DNS server information
    }
};
```