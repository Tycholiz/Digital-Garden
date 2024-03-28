
CDNs are a kind of [[cache|deploy.distributed.cache]] that comes into play for sites serving large amounts of static media. In a typical CDN setup, a request will first ask the CDN for a piece of static media; the CDN will serve that content if it has it locally available. If it isn’t available, the CDN will query the back-end servers for the file, cache it locally, and serve it to the requesting user.
- If the system you are building is not large enough to have its own CDN, you can ease a future transition by serving the static media off a separate subdomain (e.g., `static.yourservice.com`) using a lightweight HTTP server like Nginx, and cut-over the DNS from your servers to a CDN later.

- A website server normally has a caching server along with it, which will cache the webpage. All requests to the URL will retrieve the content from the cache, until the expiry time is hit. At that point, the user will make a fetch, which will see that the cached content is expired, and will then hit the server, which will retrieve the content, pass it to the caching server, which passes it to the user. All subsequent calls (from other users) will be intercepted by the caching server until it is expired (warm caching)
![](/assets/images/2021-03-11-15-50-18.png)
- A CDN provider will place servers in many locations, but some of the most important are the connection points at the edge between different networks
- Without a CDN, transit may take a slower and/or more convoluted route between source and destination
- Originally, a CDN was like a cache for static assets that don't change, like images, logos, stylesheets. Nowadays, that benefit only make up ~10% of everything a CDN can do. 
- while CDNs perform caching, not everything that performs caching is a CDN
- CDNs reduce the importance of where your web server is located, since this would theoretically only impact the timecost of the first fetch of data of each expiry cycle.
	- In practice, this is only true for data that doesn't change often. If we are talking about a Facebook news feed that updates frequently, then you are still going to be interfacing a lot with the server, making the location of your server ultimately still important. 
- The DNS plays a central role in the CDN 
- a CDN is known as a distributed internet service

![](/assets/images/2021-03-11-15-50-32.png)

If any of the following is true for your website, you should definitely opt for a CDN:
- Large amounts of traffic
- A scattered, international user-base
- Expected growth
- Lots of media content, such as images and videos

If the answer to all 4 is no, you won’t notice a difference in speed, but you will still get the benefit of [[DDoS|security.DDOS]] protection.
