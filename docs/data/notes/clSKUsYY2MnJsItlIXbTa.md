
kube-proxy is a network proxy that runs on each node in your cluster

kube-proxy maintains network rules on nodes.
- These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

kube-proxy watches and detects when there are new services, and new endpoint objects.
- it also manages iptable rules which capture traffic to the Service port and redirect it to endpoints. For each endpoint object, it adds an iptables rule which selects a Pod.

kube-proxy interfaces with the k8s API server and iptables
