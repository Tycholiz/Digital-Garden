
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

#### Context
A context is essentially the combination of a user and a cluster configuration/certificates.
- ex. referring to using ContextA could mean using User5 with Cluster2, depending on how we configure it in `~/.kube/config`

A context provides us with a way to manage and switch between different clusters and user configurations.

It consists of the following components:
- cluster
- user: an entity that interacts with the Kubernetes API
- namespace: a way to divide cluster resources between multiple users or projects.

It’s important to understand that a Kubernetes context only applies to the client side. The Kubernetes API server itself doesn’t recognize context the way it does other objects such as pods, deployments, or namespaces.

Kubernetes configuration files (typically located at `~/.kube/config` or a file specified by the `KUBECONFIG` environment variable) store context information along with other configuration details. The `kubectl` command-line tool uses this configuration file to determine which cluster, user, and namespace to interact with.

Contexts are useful in scenarios where you work with multiple Kubernetes clusters, such as development, testing, and production environments. They help ensure that your kubectl commands are directed to the intended cluster and namespace with the correct user credentials.