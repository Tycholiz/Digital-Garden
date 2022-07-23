
It is meaningless to say “X is scalable” or “Y doesn’t scale.” Rather, discussing scalability means considering questions like "If the system grows in a particular way, what are our options for coping with the growth?" and "How can we add computing resources to handle the additional load?"

A system that is designed to handle 100,000 requests per second, each 1 kB in size, looks very different from a system that is designed for 3 requests per minute, each 2 GB in size—even though the two systems have the same data through‐ put.

## Scaling Types
### Vertical Scaling (a.k.a. *scaling up*)
getting bigger machines to handle the increasing workload.
- ie. make each node more powerful (adding CPUs etc)

The cost of vertical scaling is not linear: twice as much RAM, CPU and disk space costs significantly more than 2x
- also, due to bottlenecks, a machine with 2x the resources cannot necessarily handle twice the load.

### Horizontal Scaling (a.k.a. *scaling out*)
getting many more smaller machines to handle the increase.
- ie. add more nodes

A system that has taken a horizontally scaled approach is said to be a *shared-nothing system*

- ex. Kubernetes does this by spinning up new containers
- ex. Azure functions does this by spinning up new function handlers


### Z-Axis Scaling
[source](https://microservices.io/articles/scalecube.html)