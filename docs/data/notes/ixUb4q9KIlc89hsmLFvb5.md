
A Load Balancer helps to spread the traffic across a cluster of servers
- Purpose is to improve responsiveness and availability of applications, websites or databases.

Load balancers can be placed in the network to direct server requests according to server performance and the method of traffic distribution chosen, such as round robin for example. In certain cases, network managers prefer to place load balancers outside the cluster to provide for increased horizontal scalability.

Load balancing helps you scale horizontally across an ever-increasing number of servers

The LB sits between some client and some server. Broadly speaking, then can be added at three places:
1. Between the user and the web server
2. Between web servers and an internal platform layer (e.g. application servers or cache servers)
3. Between internal platform layer (e.g. application server) and database/cache.

![](/assets/images/2021-10-12-09-10-37.png)

LB also keeps track of the status of all the resources while distributing requests
- If a server is not available to take new requests or is not responding or has elevated error rate, LB will stop sending traffic to such a server.
- Load balancers should only forward traffic to "healthy" backend servers. To monitor the health of a backend server, "health checks" regularly attempt to connect to backend servers to ensure that servers are listening. If a server fails a health check, it is automatically removed from the pool, and traffic will not be forwarded to it until it responds to the health checks again.

Since the LB can itself be a single point of failure, it's considered a good practice to have a second redundant LB to form a cluster.
- Each LB monitors the health of the other and, since both of them are equally capable of serving traffic and failure detection, in the event the main load balancer fails, the second load balancer takes over.

### Purpose of LB
- Users experience faster, uninterrupted service.
    - Users won’t have to wait for a single struggling server to finish its previous tasks. Instead, their requests are immediately passed on to a more readily available resource.
- Service providers experience less downtime and higher throughput. 
    - Even a full server failure won’t affect the end user experience as the load balancer will simply route around it to a healthy server.
- Smart load balancers provide benefits like predictive analytics that determine traffic bottlenecks before they happen.

### LB Algorithms
- *Round Robin Method* — This method cycles through a list of servers and sends each new request to the next server. When it reaches the end of the list, it starts over at the beginning. 
    - This strategy is most useful when the servers are of equal specification and there are not many persistent connections.
    - A problem with Round Robin LB is that we do not consider the server load. As a result, if a server is overloaded or slow, the LB will not stop sending new requests to that server.
    - Also, Round Robin LB doesn't consider that different requests might be more expensive than others.
- *Least Connection Method* — This method directs traffic to the server with the fewest active connections.
    - This approach is useful when there are a large number of persistent client connections which are unevenly distributed between the servers.
    - It is also useful when all requests are not equal and some may take longer time to process than others.
    - use this strategy in a web application where users can perform a variety of actions – from quick data retrieval to running long, complex reports.
- *Least Response Time Method* — This algorithm directs traffic to the server with the fewest active connections and the lowest average response time.
    - useful for applications where you have a mix of static and dynamic content. 
    - ex. a media streaming application where you have static images, text, and also video content.
- *Least Bandwidth Method* - This method selects the server that is currently serving the least amount of traffic measured in megabits per second (Mbps).
- *Weighted Round Robin Method* — The weighted round-robin scheduling is designed to better handle servers with different processing capacities. Each server is assigned a weight (an integer value that indicates the processing capacity). Servers with higher weights receive new connections before those with less weights and servers with higher weights get more connections than those with less weights.
- *IP Hash* — Under this method, a hash of the IP address of the client is calculated to redirect the request to a server.
    - This approach provides session persistence and is useful when you want to maintain the user's session on the same server. 
    - ex. e-commerce sites where users have shopping carts that need to persist would benefit from this approach.
- *Geographic Load Balancing* - This method routes traffic based on the geographic location of the user, often to the nearest or best-performing datacenter. 
    - Useful for multinational applications where you want to provide the best user experience in terms of latency. 
    - ex. global streaming services like Netflix or YouTube use this strategy.
