
Reverse proxies are servers that sit between the request-response process that retrieve resources from a server. These resources are then returned to the client, appearing as if they originated from the proxy server itself.
- ex. This might include services such as caching, security (inc. SSL), load balancing, and content optimization

A reverse proxy acts on behalf of a server, intercepting client requests and forwarding them to the appropriate backend servers.

The backends that are receiving the proxied requests are called *upstreams*
- ie. these would be your application servers/database etc.

- Proxying is typically for load balancing, to seamlessly show content from different websites, or to pass requests for processing to application servers over protocols other than HTTP.
- It's called a reverse proxy because the client makes a request, and before that request can hit the server, it's intercepted by a server (the proxy server), which gives the request what it wants, and sends (reverses) it back to the client.
- A common scenario is that we run our REST API behind a reverse proxy. Among other reasons, we might want to do this so our API server is on a different network/IP than our front-end application. Therefore, we can secure this network and only allow traffic from the reverse proxy server.
- A reverse proxy like one from Nginx can be used to implement SSL
	- [guide](https://nginx.org/en/docs/http/configuring_https_servers.html)

Example:
- Imagine we had content stored in 3 different places. We could use a reverse proxy with rewrite rules to determine where each request would go depending on the actual endpoint of where the request was sent.
- in this case, each request is forwarded on to a different origin
![](/assets/images/2021-12-07-12-13-42.png)

### Cook
#### Proxy requests to different local backends (ie. upstreams)
Imagine we want to have a RPi that exposes 3 websites to the internet, without having to specify port at the end of the URL. As it is, we can't expose them all on port 80/443. To get around this, we can configure a webserver like Apache/Nginx/Caddy as a reverse proxy to forward the requests to each specific application hosted on the RPi. With a reverse proxy configured, the webserver is able to know which requests go to which server based on the domain (or even the URL path). We can set this up by exposing the webserver to the internet via ports 80 & 443, and when the webserver receives requests, it will proxy them on to each individual app.

![](/assets/images/2024-02-28-18-29-37.png)

# E Resources
- [AWS Reverse Proxy Architecture](https://aws.amazon.com/blogs/architecture/serving-content-using-fully-managed-reverse-proxy-architecture/)
