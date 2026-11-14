
A ConfigMap is an object used to store **non-confidential** data in key-value pairs. Pods then consume ConfigMaps as environment variables, command-line arguments or as configuration files in a volume.

ConfigMaps get mounted on pods in a deployment

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:
- Inside a container command and args
- Environment variables for a container
- Add a file in read-only volume, for the application to read
- Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap