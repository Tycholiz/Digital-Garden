
# Proxy server
- A proxy server is a gateway from one network to another 
	- Ex. the Cloudflare proxy server gives the local network access to the network that holds that cached information.
- a proxy acts on behalf of the client(s), while a reverse proxy acts on behalf of the server(s)
	- When you see "proxy" think: "something that stands-in for something else."

## Caching
Another advantage of a proxy server is that its cache can serve a lot of requests. If multiple clients access a particular resource, the proxy server can cache it and serve it to all the clients without going to the remote server.

When you think about it, HTTP requests have the same benefits that pure functions do: for a given request, there is a given response. This means that if we remember all of the details of the request, we may be able to store that corresponding response, and serve that data to all subsequent identical requests. 
- The proxy server figures out whether or not it should cache data based on the headings that are found in the request. 
- a cache is used by a proxy server. Think of it as storage that the proxy server returns to the client making the request. 

When we send a request to a server for a particular resource, the proxy server intercepts the message (establishing a TCP connection between it and the browser) and checks if it has a copy of the resource cached. If it does, then it returns it. If it doesn't then it opens a new TCP connection between it and the origin server, gets the resource, then returns it to the client. The client is none the wiser that any of this has happened.

Web caches are not computation heavy, and therefore can be low-cost machines.
