A Pod is a way to represent a running process in a cluster.
- Pod refers to a pod of whales or pea pod

A pod is a collection of containers that share resources
- Though realistically, we tend to only have a single container in a Pod. We might see more than 1, but it normally isn't more than 2 or 3.

A pod is designed to run multiple cooperative processes that could be seen as a single cohesive piece of work. This is the level of abstraction that we live at in Kubernetes.

Pods are the building blocks of Kubernetes, just as containers are the building block of Docker.
- In Docker, we think in terms of processes. In Kubernetes, we think in terms of multiple processes (co-existing to perform one task)

All the containers in a pod run on the same machine.
- That is, a pod cannot be split across multiple nodes

A pod provides a way to set `.env` variables, mount storage, and feed other information into a container

A pod encapsulates one or more containers deployed together on one host, thereby sharing the same resources (of the host)
- ex. if we have 5 containers of a mongodb service deployed, and 3 of them were on the same host (ex. same machine), those 3 together would be called a Pod

Pods are not long-lived services. Even though Kubernetes is doing its best to ensure that the containers in a Pod are (almost) always up-and-running, the same cannot be said for Pods. In Kubernetes, containers are fault-tolerant, but pods are not.

- If a Pod fails, gets destroyed, or gets evicted from a Node, it will not be rescheduled.
- Similarly, if a whole node is destroyed, all the Pods on it will cease to exist.

Each pod gets its own IP address, though it is unreliable, since pods are designed to be short-lived; so the creation of a new pod would result in a new IP

When the container inside a pod exits, the pod dies too.

When a container inside a pod fails, Kubernetes will create a new container based off the same image:

```sh
$ kubectl exec -it db pkill mongod
$ kubectl get pods
```

produces (note how RESTARTS is 1):

```
NAME READY STATUS  RESTARTS AGE
db   1/1   Running 1        13m
```

Everything in a pod is tightly coupled.

The containers in a pod are not necessarily Docker containers, though it is the most common implementation.

Normally pods are not created by hand. Instead, we depend on higher level constructs like Controllers to do that for us.

Pods...
- are mortal. They are born and cannot be resurrected once they die.
- are not intended to run multiple instances of the same application,

Containers within a pod...
- share an IP address and port space, and can find each other via localhost.
- share storage space

In a pre-container world, being executed on the same physical or virtual machine would mean being executed on the same logical host.

- logical host would contain relatively tightly coupled code

### How many containers in a pod?

Even though a Pod can contain any number of containers, the most common use case is to use the **single-container-in-a-Pod** model

- Imagine we had your express api server image and a postgres image. If we put both of these in a single pod, we would no longer be able to have different numbers of containers. For instance, we could not have 2 api containers and 1 postgres container.

There are scenarios when having multiple containers in a Pod is a good idea. However, they are very specific and, in most cases, are based on one container that acts as the main service and the rest serving as side-cars.

A frequent use case is multi-container Pods used for:

- Continuous integration (CI)
- Continious Delivery (CD)
- Continuous Deployment processes (CDP)

### PodTemplate

When we create a pod, a hash of the PodTemplate is taken and it appended to the Pod name. This means that 2 pods created from identical PodTemplates on different machines will produce the same hash.

- This is also how Git SHAs work.

spec: PodTemplate is in the ReplicaSet

## Pod Scheduling

### Major components

There are 3 major components: API Server, Scheduler, Kubelet

#### API Server

Central component of the K8s cluster

- runs on the master node
  - with Minikube, both master and worker nodes are baked into the same VM. Realistically, the K8s cluster should have the two separated on different hosts.

Most of the coordination in Kubernetes consists of a component writing to the API Server resource that another component is watching. The second component will then react to changes almost immediately.

#### Scheduler

The scheduler is also running on the master node.

- Its job is to watch for unassigned pods and assign them to a node which has available resources (CPU and memory) matching Pod requirements.

#### Kubelet

[[reference|k8s.components.js.node.kubelet]]

### Process of creating a pod

ie. when running `kubectl create -f pod/db.yml`

1. kubectl (the K8s client) sends a request to the API Server, requesting the creation of a pod
2. Since the scheduler is watching the API server for new events, it detected that there is an unassigned Pod.
3. The scheduler decided which node to assign the Pod to and sent that information to the API server.
4. Kubelet is also watching the API server. It detected that the Pod was assigned to the node it is running on.
5. Kubelet sent a request to Docker requesting the creation of the containers that form the Pod. In our case, the Pod defines a single container based on the mongo image.
6. Finally, Kubelet sent a request to the API server notifying it that the Pod was created successfully.

![Pod Scheduling Sequence](/assets/images/2021-05-28-11-22-21.png)

## Pod Definition File

```yml
# using v1 of the K8s Pods API
apiVersion: v1
kind: Pod
metadata:
  name: db
  labels:
    type: db
    vendor: MongoLabs
spec:
  containers:
  - name: db
    image: mongo:3.3
    command: ["mongod"] # the command that should be executed when the container starts
    args: ["--rest", "--httpinterface"]

```
