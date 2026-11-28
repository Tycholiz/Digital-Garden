
ServiceAccount objects allow clients within the Kubernetes cluster to authenticate to the [[k8s.node.master.components.api-server]]
- A service account provides an identity for processes that run in a Pod, and maps to a ServiceAccount object.

When Pods contact the API server, they authenticate as a particular ServiceAccount. 
- There is always at least one ServiceAccount in each namespace (If you do not specify a ServiceAccount when you create a Pod, `default` is used).