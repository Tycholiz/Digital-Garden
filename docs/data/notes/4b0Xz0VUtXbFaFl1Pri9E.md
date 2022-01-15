
Ingress objects manage external access (ie. web traffic) to the applications running inside a Kubernetes cluster.
- it may seem that this use-case is already solved by [[services|k8s.objects.service]], but services don't make applications truly accessible, because we still need forwarding rules based on paths and domains, SSL termination and a number of other features.
- in a more traditional setup, we’d probably use an external proxy and a load balancer. Ingress provides an API that allows us to accomplish these things, in addition to a few other features we expect from a dynamic cluster.

Ingresses act as traffic cop, routing traffic from outside the cluster into destination points within the cluster.
- One single external ingress point can accept traffic destined to many different internal services.

The ingress controller must specify a provider. Out of the box, Kubernetes supports Nginx, AWS, GCE, but we can also get more controllers, like Apache.

With Ingress, we can configure an external load balancer directly from Kubernetes

Don't confuse ingress objects with ingress controllers
- Ingresses (or ingress objects) are the objects that use the controller to define an http load balancer to facilitate web traffic in and out of the cluster.
