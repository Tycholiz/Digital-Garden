
[Kubernetes dashboard](https://github.com/kubernetes/dashboard) gives us a GUI where we can perform many of the same actions that [[k8s.kubectl]] lets us.

To run it, use the manifest file:
`kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml`

- It’s a single manifest file that will create a deployment, service, and the necessary permissions for the dashboard to run. All these resources will be created in the kubernetes-dashboard namespace.