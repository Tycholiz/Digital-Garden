
A node is a worker machine in Kubernetes, responsible for running containers and other workloads. Nodes are part of a cluster and communicate with the master to receive instructions on deploying and managing containers.

There are 2 types of Node in Kubernetes:
- A single [[k8s.node.master]] (a.k.a Control Plane)
- One or more [[k8s.node.worker]] (a.k.a. compute machines)

Cumulatively these nodes make up a *Kubernetes Cluster*.
- At a minimum, a cluster contains a control plane and one or more compute machines (nodes).

### Node Pool
A node pool is a subset of nodes within a cluster that share the same configuration settings. 

These settings include the machine type, disk type and size, and network settings. 

Node pools provide a way to group nodes based on specific requirements.