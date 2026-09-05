
The Master node (or, control plane) is considered the brain of the cluster
- we send desired state in the form of a `yml` file to the master node, and it figures out how to achieve that through the worker nodes
    - this comparison of desired state to actual state is always happening, and the master node is constantly working to make sure the actual matches the desired.

The control plane is accessible by the internet via a URL.

The control plane is the brain of the cluster. It is responsible for maintaining the desired state of the cluster, such as which applications are running and which container images they use.

Nodes actually run the applications and workloads, and are spun up during the cluster creation process.

The cluster enables Kubernetes to schedule and run containers across a group of machines.
- The heart of this whole idea is that Kubernetes containers aren’t tied to individual machines. Rather, they’re abstracted across the cluster.

A Kubernetes cluster has a desired state, which defines which applications or other workloads should be running, along with which images they use, which resources should be made available for them, and other such configuration details.
- This desired state is described in a YAML file

The control plane includes the K8s API server, storage, scheduler and core [[resource controllers|k8s.controllers]]
- it is therefore responsible for determining what gets run on all of the cluster's nodes.
