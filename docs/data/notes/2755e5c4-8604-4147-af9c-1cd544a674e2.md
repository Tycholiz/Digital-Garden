
Kubelet runs on each node. Its primary function is to make sure that assigned pods are running on the node.
- It watches for any new Pod assignments for the node. If a Pod is assigned to the node Kubelet is running on, it will pull the Pod definition and use it to create containers through Docker or any other supported container engine.
