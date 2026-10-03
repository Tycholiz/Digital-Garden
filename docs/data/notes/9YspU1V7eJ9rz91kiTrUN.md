
#### ClusterIP (default)
exposes the port only inside the cluster, making it inaccessible from the outside world.
- Therefore, use this service when we want to enable communication between pods, while preventing external access.
- With ClusterIP, all the Pods in the cluster can access the `TargetPort` (the port of the associated Pod that will receive all the requests)
- used when we only need to give other applications that are running inside our cluster access to our pods.
  - ex. If we have 3 replicas of application `app-a` and another application `app-b` needs a stable endpoint to access these replicas, we could create `service-a` using the ClusterIP type.

#### NodePort
Exposes ports to all the nodes
- If the service is of type NodePort, ports will be available both within the cluster as well as from outside by sending requests to any of the nodes.
- NodePort is an extension of ClusterIP
  - therefore everything we can do with a ClusterIP, we can also do with a NodePort service. 
- NodePort is the port which we can use to access the Service and, therefore, the Pods from the outside world.
  - It works by opening a port on all the worker nodes we have in our cluster, and then redirecting requests received on that port to the correct location, even if the pod we are trying to reach is physically running on a different node.
  - in most cases the port should be randomly generated to avoid clashes

![](/assets/images/2022-02-28-22-03-11.png)
- here, external clients would be able to access either http://node1-ip:30001 or http://node2-ip:30001
- When one of the nodes receives a request on this port, it will find our service that will then be able to decide which pod should receive the request (even if the pod is physically running in another node)

#### LoadBalancer
The LoadBalancer type is an extension of the NodePort type, but it will try to provision a Load Balancer on the cloud provider we are running.
- Therefore, it is only useful when combined with cloud provider’s load balancer.
- ex. if our Kubernetes cluster is running on AWS when we create a LoadBalancer service, it would automatically create an ELB
- If we have a cloud provider already, this is probably the easiest way to expose an application running in Kubernetes to the outside world.

The way it works is pretty similar to the other service types, but instead of having to connect to a worker node IP and port, we can send requests to this Load Balancer, and it will route them to our pods the same way.

![](/assets/images/2022-02-28-22-09-50.png)

#### ExternalName
maps a service to an external address (e.g., `kubernetes.io`)
- This service type is a little bit different, as it is not used to provide a stable endpoint to an internal service but to an external one.
  - ex. we have a database running at `my-db.company.com`. Since we declare this as the ExternalName, our pods can talk to this service without having to know the real address where our database is running. Now if this database changes location, the service is the only place we need to change. Pods are not affected.
- This service has limited usage.