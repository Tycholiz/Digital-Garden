
Minikube commands are almost exactly the same as those from Docker Machine which, on the other hand, are similar to those from Vagrant.

`minikube start --vm-driver=virtualbox`
- creates a new VM based on the Minikube image, and deploys the necessary Kubernetes components into it.
- The VM will get configured with Docker and Kubernetes via a single binary called *localkube*.
- This image includes several binaries, including Docker engine
- after creating a new VM, we'll need to connect the docker client to the docker server with `eval $(minikube docker-env)`

`minikube dashboard`
- open a browser-based UI

`minikube docker-env`
- output the docker environment variables to console
- these env variables are relevant for the docker engine running in the image

`minikube stop`
- stop the cluster, but preserve cluster state and data
