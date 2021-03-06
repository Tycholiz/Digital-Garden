
A Kubernetes cluster is a set of node machines for running containerized applications

At a minimum, a cluster contains a control plane and one or more compute machines (nodes).
- The control plane is the brain of the cluster. It is responsible for maintaining the desired state of the cluster, such as which applications are running and which container images they use.
- Nodes actually run the applications and workloads.

The cluster enables Kubernetes to schedule and run containers across a group of machines.
- note: this could mean multiple VMs on the same machine
- The heart of this whole idea is that Kubernetes containers aren’t tied to individual machines. Rather, they’re abstracted across the cluster.

A Kubernetes cluster has a desired state, which defines which applications or other workloads should be running, along with which images they use, which resources should be made available for them, and other such configuration details.
- This desired state is described in a YAML file
