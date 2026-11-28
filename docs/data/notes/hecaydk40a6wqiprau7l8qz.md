
The core of Kubernetes' [[control plane|k8s.node.master]] is the API server.

The API that is exposed allows end users, different parts of your cluster, and external components to communicate with one another.

Through the API, we can query and manipulate the state of [[API objects|k8s.objects]] in Kubernetes (for example: Pods, Namespaces, [[ConfigMaps|k8s.objects.configmap]], and Events).

Most of the time we use [[kubectl|k8s.kubectl]] which interacts with the API server for us, but we can also interact with it directly.

https://kubernetes.io/docs/concepts/overview/kubernetes-api/