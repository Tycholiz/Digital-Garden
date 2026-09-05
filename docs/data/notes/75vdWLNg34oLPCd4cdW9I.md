
Also known simply as "proxies"
- From the server's perspective, requests came from the proxy server (in this case, the proxy server is just passing along the request made by the client)
- It's called a forward proxy because when a request is detected, the proxy server intercepts it, and forwards it on to the next destination

A forward proxy is typically used to retrieve resources on behalf of clients from other servers on the internet
- Clients send their requests to the forward proxy, which then forwards those requests to the target servers. 

The forward proxy is usually used to provide:
- Anonymity: The client's identity is hidden from the target server.
- Caching: The forward proxy can cache frequently requested resources, improving performance and reducing bandwidth usage.
- Content Filtering: The forward proxy can filter and block certain types of content based on configured rules.
- Access Control: The forward proxy can enforce access policies, allowing or denying access to certain websites or resources.

## Web Proxy Server
- It's common to run web apps behind a proxy such as NGINX
	- Nginx listens on port 80, then forwards traffic to the app on another port, like 4000
- An early limiting factor of Nginx that will be encountered will be related to the number of open sockets that the Nginx server can handle. The result is delays on the client side. 
	- the next limiting factor would be the number of ports available (there are only 64k). These 64,000 ports can handle 500 requests/second 
