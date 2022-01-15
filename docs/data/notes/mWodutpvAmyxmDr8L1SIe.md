
DaemonSets have many uses
- One common pattern is to use a DaemonSet to install/configure software on each host node.

DaemonSets provide a way to ensure that a copy of a pod is running on every node in the cluster. As a cluster grows and shrinks, the DaemonSet spreads these specially labeled Pods across all of the nodes.
