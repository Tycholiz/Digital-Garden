
## Overview
- Nginx has 1 master process, and multiple worker processes.
	- The master's job is to read configuration files, and to manage the worker processes.
	- The worker processes handle the requests.
- Nginx uses an event-based model to distribute requests among workers
- The # of workers is specified in the config file, and may either be fixed, or adjustable based on how many cores the CPU has. 

### Config file
- The nginx config file `nginx.conf` is stored either in `/usr/local/nginx/conf`, `/etc/nginx`, or `/usr/local/etc/nginx`.
- nginx consists of modules which are controlled by directives specified in the configuration file
	- Directives can either be simple directives or block directives
		- simple ends with `;`, block uses `{}`
- If a directive can have other directives inside, it is called a Context 
	- ex. `events`, `http`, `server`, `location`
- If a directive is not placed within a Context, then it is considered to be in the Main Context. 
	- The `events` and `http` directives reside in the Main Context, `server` in `http`, and `location` in `server`.

## Blocks
- Nginx divides the configurations meant to serve different content into Blocks, which live in a hierarchical structure.
- Each time a client request is made to the server, Nginx begins a process of determining which hierarchical block should be used to handle the request.

### Server Block
- Defines a virtual server used to handle requests of a defined type
	- each Server Block functions as a separate virtual web server instance
- Based on the domain name, port and IP address requested, we can configure multiple server blocks to handle each combination.
- The `server_name` and `listen` directives are used to determine which server block should be used to fulfill a request. They are used to bind to tcp sockets.
	- With `listen`, we can use a lone IP, a lone port, or a combo of the two. If we only specify one, then defaults are used
		- default port: 80
		- default IP: 0.0.0.0
	- the `server_name` directive is only evaluated when nginx needs to distinguish between server blocks that match to the same level of specificity in the `listen` directive. Put another way, it is a "specificity tie-breaker" 
		- in other words, if `example.com` is hosted on `192.168.1.10:80`, a request will always be served by a server block that specifies `listen 192.168.1.10`, even if there is another server block that specifies `server_name example.com`
	- Finally, if further specificity is needed, then the Host header from the request (which contains the URI that the client was trying to reach) is used. 
		- When using wildcard matching, the longest match beginning with a wildcard is used
			- ex. if the request has a Host header of `www.example.com`, and we have 3 server blocks with `server_name` of `*.example.com`, `www.example.*` and `*.com`, `*.example.com` would win out.
- With server blocks, we can run more than one website on a single host
- in Apache, called *VirtualHost* 

### Location Block
- Lives within a Server Block (or nested in other location blocks).
- Determine how Nginx should handle the part of the request that comes after IP:port (ie. the URI).
- Similar to how Nginx has a specificity-based process for determining which server block will process the request, Nginx has an algorithm to determine which location block within the server should be used for handling requests.
- Location blocks take the following form:
```
location <optional_modifier> <location_match> {
}
```
- The `location_match` defines what Nginx should check the request URI against.
- The `optional_modifier` affects the way Nginx will attempt to match the location block.
	- ex. check for prefix match (default), check for exact match (`=`), check for case-sensitive Regex (`~`)
- The URI specified after `location` will be added to the path specified in the *root directive*
	- ex. if we specify `root /var/www/` and the location block specifies `/images/`, then the path to the requested file on the local FS will be `/var/www/images`
- ex. Imagine we had a server block:
```
server {
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
}
```
in response to a request with URI starting with `/images/`, the server will send files from the `/data/images` directory. 

## Tasks of Nginx
### Serving Static Content
- Nginx can be configured to serve static content, such as HTML and images.
- this involves setting up of a server block inside the http block with two location blocks.
	- multiple server blocks are able to be added, each specifying a different port.
```
http {
	server {
		
	}
}
```

### Reverse Proxy Server
- When Nginx proxies a request, it sends the request to a specified proxied server, fetches the response, and sends it back to the client
	- it is possible to proxy requests to another HTTP server (eg. another Nginx server) or to a non-HTTP server (eg. express.js)
		- We use a specified protocol like FastCGI to do this
- We can establish a proxy server by using the `proxy_pass` directive within a *location block*.
	- The value of `proxy_pass` is the address of the proxy server:
```
location /some/path/ {
    proxy_pass http://www.example.com/link/;
}
```
- In this config, all requests processed to `/some/path/` to be sent to the proxy server at `http://www.example.com/link/`.
	- ex. the request with the URI of `/some/path/page.html` will be proxied to `http://www.example.com/link/page.html`
- to pass a request to a non-HTTP server, the appropriate `*_pass` directive should be used
	- ex. `fastcgi_pass`

## Debugging
- upon changing nginx.conf, we need to reload the nginx server with `nginx -s reload`.
- logs are stored at either `/usr/local/nginx/logs` or `/var/log/nginx`

### Pitfalls
- A *root directive* should occur outside of the location block. We can then use another *root directive* within a *location block* if we want to override it.
	- Conversely, if you were to add a root to every location block then a location block that isn’t matched will have no root. Therefore, it is important that a root directive occur prior to your location blocks, which can then override this directive if they need to.
- [source](https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/)
