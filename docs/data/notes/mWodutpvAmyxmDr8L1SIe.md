
A DaemonSet is a type of controller that ensures that a copy of a specific Pod is running on each node in the cluster. 
- As a cluster grows and shrinks, the DaemonSet spreads these specially labeled Pods across all of the nodes.

The primary purpose of a DaemonSet is to deploy and manage background services, daemons, or other system-level processes that should run on every node in the cluster.

DaemonSets have many uses
- One common pattern is to use a DaemonSet to install/configure software on each host node in the cluster.
  - ex. we can use a DaemonSet to install a [[Datadog Agent|datadog#datadog-agent,1]] in our cluster

Common use cases for DaemonSets include:
- Monitoring Agents: Deploying monitoring agents on every node to collect metrics and logs.
- Storage Daemons: Running storage-related daemons (e.g., distributed storage daemons) on each node.
- Network Daemons: Deploying network-related services (e.g., proxies or VPN agents) on every node.

### Example
In this example, the DaemonSet ensures that one Pod with the specified template runs on each node in the cluster, labeled with app: example. The DaemonSet automatically adjusts as nodes are added or removed from the cluster.

```yml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: example-daemonset
spec:
  selector:
    matchLabels:
      app: example
  template:
    metadata:
      labels:
        app: example
    spec:
      containers:
      - name: example-container
        image: example-image:latest
```