
Usually we just call these *nodes*, and refer to the [[master node|k8s.node.master]] as the *control plane*.

The worker nodes contain the services necessary to run Pods.
- meaning that it hosts [[pods|k8s.objects.pod]]

Multiple worker nodes could either be distinct machines, or could be VMs on the same machine.

The worker nodes have the following characteristics:
- we should never need to interact with it directly
- should be easily replaceable
- multiple applicatons can run on the same node
- one application can have multiple replicas spread across many nodes

A worker node runs the services necessary to support the containers that make up your cluster's workloads.
- this includes the container runtime, and the K8s node-agent [[Kubelet|k8s.node.worker.components.kubelet]]

### Components running in a node
- [[k8s.node.worker.components.kubelet]]
- [[k8s.node.worker.components.kube-proxy]]
- Container runtime (the software the runs the [[docker.containers]])

### Adding nodes to the API server
There are two main ways:
1. The kubelet on a node self-registers to the control plane
2. The developer manually adds a Node object