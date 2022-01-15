
#### ClusterIP (default)
exposes the port only inside the cluster, making it inaccessible from the outside world.
- Therefore, use this service when we want to enable communication between pods, while preventing external access.
- With ClusterIP, all the Pods in the cluster can access the `TargetPort` (the port of the associated Pod that will receive all the requests)

#### NodePort
exposes ports to all the nodes
- If the service is of type NodePort, ports will be available both within the cluster as well as from outside by sending requests to any of the nodes.
- If NodePort is used, ClusterIP will be created automatically.
- NodePort is the port which we can use to access the Service and, therefore, the Pods from outside the cluster
  - in most cases the port should be randomly generated to avoid clashes

#### LoadBalancer
only useful when combined with cloud provider’s load balancer.

#### ExternalName
maps a service to an external address (e.g., kubernetes.io)
- This service has limited usage.
