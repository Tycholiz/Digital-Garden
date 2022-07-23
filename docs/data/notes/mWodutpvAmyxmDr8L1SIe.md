
DaemonSets have many uses
- One common pattern is to use a DaemonSet to install/configure software on each host node in the cluster.
  - ex. we can use a DaemonSet to install a [[Datadog Agent|datadog#datadog-agent,1]] in our cluster

Other typical uses of a DaemonSet are:
- running a cluster storage daemon on every node
- running a logs collection daemon on every node
- running a node monitoring daemon on every node

DaemonSets provide a way to ensure that a copy of a pod is running on every node in the cluster. As a cluster grows and shrinks, the DaemonSet spreads these specially labeled Pods across all of the nodes.
