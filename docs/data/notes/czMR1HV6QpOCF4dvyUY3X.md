
Minikube is a virtual machine that runs locally and has the necessary Kubernetes components deployed into it. The VM will get configured with Docker and Kubernetes via a single binary called localkube.

If we were using Docker Swarm (instead of Kubernetes), we'd be able to `docker swarm init` to create a local Docker Swarm Cluster. Minikube is a service that allows us to have this level of simplicity with Kubernetes

In Minikube, there's only 1 server that acts as both the master and the node

### localkube
Provides a single-node cluster running locally on our machine
The localkube library provides everything needed to run a Kubernetes cluster locally
