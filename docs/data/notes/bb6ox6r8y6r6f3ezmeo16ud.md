
The master node's components make global decisions about the cluster (for example, scheduling), as well as detecting and responding to cluster events (for example, starting up a new pod when a deployment's replicas field is unsatisfied).

Master node components can be run on any machine in the cluster. However, for simplicity, set up scripts typically start all control plane components on the same machine.

### etcd
etcd is the heart of the Kubernetes cluster.
- it is a key value store used as Kubernetes' backing store for all cluster data.
- [[k8s.node.master.components.etcd]]

### kube-apiserver
exposes the Kubernetes API
- The API server is the front end for the Kubernetes control plane.
[[k8s.node.master.components.api-server]]

### kube-scheduler
A component that watches for newly created Pods with no assigned node, and selects a node for them to run on.

### kube-controller-manager
runs controller processes.