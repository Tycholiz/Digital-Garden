
# Forward Proxy (or simply Proxy)
- From the server's perspective, requests came from the proxy server (in this case, the proxy server is just passing along the request made by the client)
- It's called a forward proxy because when a request is detected, the proxy server intercepts it, and forwards it on to the next destination

## Web Proxy Server
- It's common to run web apps behind a proxy such as NGINX
	- Nginx listens on port 80, then forwards traffic to the app on another port, like 4000
- An early limiting factor of Nginx that will be encountered will be related to the number of open sockets that the Nginx server can handle. The result is delays on the client side. 
	- the next limiting factor would be the number of ports available (there are only 64k). These 64,000 ports can handle 500 requests/second 
