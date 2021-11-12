
A namespace is a virtual cluster. It allows Kubernetes to manage multiple clusters (which could be multiple different application within the same product) within the same physical cluster

Namespaces provide us a way to group and segment pods, rcs, volumes, and secrets
- A namespace functions as a grouping mechanism inside of Kubernetes. Services, pods, replication controllers,
and volumes can easily cooperate within a namespace, and the namespace provides a degree of isolation from
other parts of the cluster.
