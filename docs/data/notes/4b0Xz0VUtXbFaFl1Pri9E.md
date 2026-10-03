
### What is it?
Ingress objects manage external access (e.g. web traffic via http routes) to the applications running inside a Kubernetes cluster. They are essentially a collection of rules that allow inbound connections to reach the services within a K8s cluster.
- an ingress can be seen as an abstraction layer on top of routing.

You can think of an Ingress as something that sits in front of several services, and based on some rules that you define, it will decide which [[service|k8s.objects.service]] a given request should be sent to.

Ingresses act as traffic cop, routing traffic from outside the cluster into destination points within the cluster.
- One single external ingress point can accept traffic destined to many different internal services.

Don't confuse Ingress objects with Ingress controllers
- Ingresses (a.k.a. Ingress objects) are the objects that use the controller to define an http load balancer to facilitate web traffic in and out of the cluster.
  - therefore an ingress controller is a dependency of an ingress
- An Ingress controller is responsible for fulfilling the Ingress, usually with a loadbalancer.
  - e.g. `ingress-nginx`, `kubernetes-ingress`

### Why do we need it?
it may seem that the use-case of allowing external traffic to the cluster is already solved by [[services|k8s.objects.service]], but services don't make applications truly accessible, because we still need forwarding rules based on paths and domains, SSL termination and a number of other features.
- in a more traditional setup, we’d probably use an external proxy and a load balancer. Ingress provides an API that allows us to accomplish these things, in addition to a few other features we expect from a dynamic cluster.
- You can configure an ingress to serve as the sole entry point to your Kubernetes cluster.
  - doing this would involve changing the service type of your services in the cluster to be of type [[ClusterIP|k8s.objects.service.types#clusterip-default,1:#*]]

We can combine an ingress with an external tool like [ExternalDNS](https://github.com/kubernetes-sigs/external-dns), which abstracts some of the DNS duties away from us.
- in a broad sense, ExternalDNS allows us to control DNS records dynamically via K8S resources. It does this while being agnostic to the specific DNS provider (ie. server) that you are using (like [[aws.svc.route53]] or Google Cloud DNS). 
  - Therefore, it is not a DNS server itself.
  - e.g. ExternalDNS can make Kubernetes resources discoverable via public DNS servers

### How does it work?
In order for an Ingress resource to work we need to have an Ingress Controller running in our cluster. 
- This controller is what will decide what happens when you create a new Ingress
    - ex. [Nginx Ingress Controller](https://www.nginx.com/products/nginx-ingress-controller)

The Ingress controller must specify a provider. Out of the box, Kubernetes supports Nginx, AWS, GCE, but we can also get more controllers, like Apache.

With Ingress, we can configure an external load balancer directly from Kubernetes

### Example
`ingress.yml`
```yml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-ingress
  annotations:
    # We are defining this annotation to prevent nginx
    # from redirecting requests to `https` for now
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
    - host: nginx.local.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nginx-svc
                port:
                  number: 1234
          - path: /hello
            pathType: Prefix
            backend:
              service:
                name: hellok8s-svc
                port:
                  number: 2345
    - host: hello.local.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: hellok8s-svc
                port:
                  number: 3456
```

This gets applied with the following command
```sh
kubectl apply -f ingress.yml
```

We now have an ingress listening on `localhost:80`
- here, we are defining a single rule that says that requests sent to the path "/" will be sent to our nginx-svc service, using the port 1234.
- accessing the above URL, we should see the default nginx page.
![](/assets/images/2022-02-28-22-33-06.png)
- naturally we can add multiple paths, otherwise it wouldn't be much different from the [[LoadBalancer service type|k8s.objects.service.types#loadbalancer,1:#*]].
  - now, depending on the path the ingress receives, it sends the request to different services.

- also, we were able to define subdomains.
