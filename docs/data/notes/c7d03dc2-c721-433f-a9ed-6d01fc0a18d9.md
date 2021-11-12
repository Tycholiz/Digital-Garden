
The endpoint controller watches the API server for new service events. When it detects that there is a new Service object, it creates endpoint objects with the same name as the Service, and it uses Service selector to identify endpoints (eg. the IP and port of the pods)

The endpoint controller exists in the Kubernetes master node, and interfaces with the k8s API server
