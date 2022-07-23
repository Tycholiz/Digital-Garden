
`kubectl` is used to manage a cluster and applications running inside it.
- When we send commands with `kubectl`, we are sending commands to the master node of the cluster.
- `kubectl` is a Kubernetes client that connects with the Kubernetes API, much like how psql is a client to a postgres server

Most often, we provide information to `kubectl` in the form of a `.yml` file.
- kubectl converts the information to JSON when making the API request.

Kubectl also supports the management of Kubernetes [[objects|k8s.objects]] using a [[kustomization|k8s.tools.kustomize]] file

### Kubeconfig files
`kubectl` uses kubeconfig files to find the information it needs to choose a cluster and communicate with the API server of a cluster.
- the kubeconfig file is used to configure access to clusters
- note: there is no single kubeconfig file; it is a generic way of referring to configuration files.

By default, `kubectl` looks for a file named config in the `$HOME/.kube` directory
