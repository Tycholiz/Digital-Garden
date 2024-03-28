
It is meaningless to say “X is scalable” or “Y doesn’t scale.” Rather, discussing scalability means considering questions like "If the system grows in a particular way, what are our options for coping with the growth?" and "How can we add computing resources to handle the additional load?"

A system that is designed to handle 100,000 requests per second, each 1 kB in size, looks very different from a system that is designed for 3 requests per minute, each 2 GB in size—even though the two systems have the same data throughput.
- the 100,000 requests per second system must optimize for low [[latency|deploy.performance#latency]] and high throughput. This means designing the system to minimize the time it takes to process each request and maximizing the number of requests that can be processed simultaneously. In this case, sacrificing storage efficiency may be tolerable.
    - You might achieve this system by using techniques like [[load balancing|deploy.distributed.load-balancer]], horizontal scaling, and [[caching|general.arch.cache]].
- the 3 requests per minute system must optimize for storage and retrieval. This means designing the system to efficiently store and retrieve large data objects, and to handle the performance requirements of large data transfers. In this case, sacrificing latency and throughput may be tolerable.
    - You might achieve this system by using techniques like sharding, compression, and content delivery networks.

## Scaling Types
### Vertical Scaling (a.k.a. *scaling up*)
getting bigger machines to handle the increasing workload.
- ie. make each node more powerful (adding CPUs etc)

The cost of vertical scaling is not linear: twice as much RAM, CPU and disk space costs significantly more than 2x
- also, due to bottlenecks, a machine with 2x the resources cannot necessarily handle twice the load.

A concern with vertical scaling might be the issue of [[locking|deploy.distributed.locks]], in cases where multiple clients are trying to modify the data simultaneously.

### Horizontal Scaling (a.k.a. *scaling out*)
getting many more smaller machines to handle the increase.
- ie. add more nodes

A system that has taken a horizontally scaled approach is said to be a *shared-nothing system*

- ex. Kubernetes does this by spinning up new containers
- ex. Serverless functions do this by spinning up new function handler instances
- ex. Database can do this by [[sharding|db.distributed.partitioning.sharding]]

### Z-Axis Scaling
[source](https://microservices.io/articles/scalecube.html)