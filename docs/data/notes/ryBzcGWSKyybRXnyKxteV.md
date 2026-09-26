
## What is it?
A Service is an abstraction which defines a logical set of [[pods|k8s.objects.pod]], and the policy by which they can be accessed.
- this "set" is determined by a *selector*
- a Service is what allows our consumer pods to not care about which provider pods they are getting data from. In other words, without Services, we'd have to know the IP addresses to connect to. With Services, we (as the consumer pod) get to just say "connect me to any pod within that Service".

Services enable communication between pods
- Without services, the only communication available by default is between containers within a single pod (via localhost)
  - this is because pods by nature are ephemeral. While true that a pod could communicate to another pod by knowing its IP address, once that pod is destroyed and recreated it's given a new IP and thus no longer reachable by the previous means.

Services should not be used to enable external access to an application.

While [[Pods|k8s.objects.pod]] come and go, Services provide a stable endpoint to access them. 
- therefore, we generally never send requests directly to a pod but always through a service.
- The link between services and pods happens through labels. Any pod that has the labels defined in the service’s selector can receive requests sent to that service.

Services are based on resources.
- ex. we create a service based on Pods through a ReplicaSet. Put another way, a service is created by exposing the ReplicaSet. This is why we specify the resource type when we are running `kubectl expose`

A service is an abstract way to expose an application (running on a set of Pods) as a network service.
- this decouples work definitions from the pods
- can be thought of as a logical set of pods and a policy by which to access them
  - the front end shouldn't care which backend it uses. Services allow us to achieve this decoupling

A service sits in front of the [[pods|k8s.objects.pod]] and takes care of receiving requests and delivering them to all the pods that are behind it.

Without a service object,
- we'd need to use `port-forward` to redirect traffic from our local machine to a pod. Otherwise, a user that doesn’t have access to our Kubernetes cluster is not able to access this application
- pods can't communicate with each other, short of hard-coding their IP address (which resets whenever the pod is re-created)
- requests only set sent to a single pod

A service provides access to pods from inside the cluster ([[Kube Proxy|k8s.node.worker.components.kube-proxy]]) or from outside the cluster ([[Kube DNS|k8s.addons.kube-dns]])

A service tells the rest of the Kubernetes environment (including other pods and replication controllers) what services your application provides. While pods come and go, the service IP addresses and ports remain the same.  Other applications can find your service through Kurbernetes service discovery

Since pods of a single service can exist on different machines, it makes sense for us to be able to interact with the service itself, so that we can orchestrate activities between all containers that are part of a service(?)

When a service is created, it inherits all of the labels of the resource type (eg. ReplicaSet) that the service is based on.
- The service is not directly associated with any controller (eg. ReplicaSet), but rather it is associated with the underlying Pods via the matching labels.

The problem with services is that each application can be reached through a different port, and we cannot expect users to know the port of each service in our cluster.
- it is a bad practice to publish fixed ports through Services, because it is likely to result in conflicts or, at the very least, create the additional burden of carefully keeping track of which port belongs to which Service.

## Service Type
[[k8s.objects.service.types]]

## Service discovery
Given that we have a few applications running in our cluster, each backed by a Kubernetes service providing a stable endpoint that we can use to reach them, we still need a way to actually find these services. That is, if `app-a` wants to talk to `app-b` using `service-b`, how does it know where it should send requests to?

Services can be accessed by hard-coding the IP address, but preferably, they can be discovered through two principal modes:
- DNS ([[k8s.addons.kube-dns]])
  - ex. If you create a service called `service-a`, Kubernetes will add an entry for this service in its DNS, so any pod will be able to call, for example, `http://service-a:4567`. That will be correctly resolved to the service’s IP
  - DNS is the easier approach
- Injected environment variables
  - since the env variables are never updated, they aren't as reliable as the DNS approach

Kubernetes converts Service names into DNS's and adds them to the DNS server.
- This is a cluster add-on that is already set up by Minikube.

### Services and Env Variables

Every Pod gets environment variables for each of the active Services
![[dendron://code/k8s.kubectl.cli#list-env-variables-in-a-pod,1:#*]]

Env provide a reference we can use to connect to a Service and, therefore to the related Pods.

Through the service IP (`kubectl describe svc <servicename>`), we can access the service externally. This IP matches the values of the environment variables `GO_DEMO_2_DB_*` and `GO_DEMO_2_DB_SERVICE_HOST`.

### Service discovery breakdown

1. When the api container go-demo-2 tries to connect with the go-demo-2-db Service, it looks at the nameserver configured in /etc/resolv.conf.
   - kubelet configured the nameserver with the kube-dns Service IP (10.96.0.10) during the Pod scheduling process.
2. The container queries the DNS server listening to port 53. go-demo-2-db DNS gets resolved to the service IP 10.0.0.19.
   - This DNS record was added by kube-dns during the service creation process.
3. The container uses the service IP which forwards requests through the iptables rules.
   - They were added by kube-proxy during Service and Endpoint creation process.
4. Since we only have one replica of the go-demo-2-db Pod, iptables forwards requests to just one endpoint.
   - If we had multiple replicas, iptables would act as a load balancer and forward requests randomly among Endpoints of the Service

![](/assets/images/2021-06-01-08-40-26.png)

* * *

### Motivation

Each Pod gets its own IP address, however in a Deployment, the set of Pods running in one moment in time could be different from the set of Pods running that application a moment later.

This leads to a problem: if some set of Pods (call them “backends”) provides functionality to other Pods (call them “frontends”) inside your cluster, how do the frontends find out and keep track of which IP address to connect to, so that the frontend can use the backend part of the workload?

### Under the hood — Adding a service

1. In running `kubectl expose rs -f rs/go-demo-2.yml`, Kubernetes client (kubectl) sent a request to the API server requesting the creation of the Service based on Pods created through the go-demo-2 ReplicaSet.
2. Endpoint controller is watching the API server for new service events. It detected that there is a new Service object.
3. Endpoint controller created endpoint objects with the same name as the Service, and it used Service selector to identify endpoints (in this case the IP and the port of go-demo-2 Pods).
4. kube-proxy is watching for service and endpoint objects. It detected that there is a new Service and a new endpoint object.
5. kube-proxy added iptables rules which capture traffic to the Service port and redirect it to endpoints. For each endpoint object, it adds iptables rule which selects a Pod.
6. The kube-dns add-on is watching for Service. It detected that there is a new service.
7. The kube-dns added db's record to the dns server (skydns).
   ![](/assets/images/2021-05-31-10-06-52.png)
   ![](/assets/images/2021-05-31-21-38-25.png)

* * *

### Selector

A service contains selector labels which are used to establish communication with the Pods containing the matching labels. There is no relation between the service and the ReplicaSet; both reference Pods through labels.

The selector is used by the Service to know which Pods should receive requests. It works in the same way as ReplicaSet selectors. In this case, we defined that the service should forward requests to Pods with labels type set to backend and service set to go-demo. Those two labels are set in the Pods spec of the ReplicaSet.

```yaml
selector:
  type: backend
  service: go-demo-2
```

### Request forwarding

Each Pod has a unique IP. Incoming requests will be forwarded on to Pods in a round-robin style, that is similar to load balancing.

## Example
```yml
apiVersion: v1
kind: Service
metadata:
  name: hellok8s-svc
spec:
  type: NodePort
  selector:
    app: hellok8s
  ports:
  - port: 4567
    nodePort: 30001
```

Of note here, we have:
- `kind: Service`
- give it a name with `name: kellok8s-svc`
- `spec` section
