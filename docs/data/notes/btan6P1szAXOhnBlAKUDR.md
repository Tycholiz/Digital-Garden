
Since Kubernetes is distributed, it needs a distributed database (running across multiple machines at a time) that is easy to store data across the cluster and watch for changes to that data. etcd is the database to handle this.

etcd can be thought of as the heart of the Kubernetes cluster. The API server is a stateless REST/authentication endpoint in front of etcd, and then every other component works by talking to etcd through the API server.

etcd manages a lot of the tricky problems in running a distributed database – like race conditions and networking – and saves Kubernetes from worrying about it.

Kubernetes uses etcd as a key-value database store. It stores the configuration of the Kubernetes cluster in etcd.
- It also stores the actual state of the system and the desired state of the system in etcd.
- It then uses etcd’s watch functionality to monitor changes to either of these two things. If they diverge, Kubernetes makes changes to reconcile the actual state and the desired state.

Everything in K8s other than etcd is stateless. If etcd isn't running, you can't make any changes to your Kubernetes cluster (though existing services will continue running!).
- this means that if have the etcd data, we effectively have a snapshot of our entire cluster, meaning that in the event of a failure, we can simply restore it.

If any of the K8s core components (e.g. API server, controller manager, scheduler etc.) have to restart, all they need to do to continue operating seamlessly is read the relevant state in etcd.

Also...
- Anything you might read from a kubectl get xyz command is stored in etcd.
- Any change you make via kubectl create will cause an entry in etcd to be updated.
- Any node crashing or process dying causes values in etcd to be changed.
- The set of processes that make up Kubernetes use etcd to store data and notify each other of changes.

If your Kubernetes cluster uses etcd as its backing store, make sure you have a back up plan for those data.