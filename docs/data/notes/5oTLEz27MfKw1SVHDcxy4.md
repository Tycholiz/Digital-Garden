
Docker engine is a tool that lets us install Docker Engine on virtual hosts and manage those hosts
	- Therefore, it is something we install on our own computer.
	- with the **driver** concept, we can deploy to 3rd party cloud services, like AWS, DigitalOcean, or even VirtualBox on your local machine
- *docker-machine* commands let us start, inspect, stop, and restart hosts, as well as configure a docker client to talk to the hosts.
- it allows us to control the docker engine of a VM created using docker-machine
- The main reason you would use docker-machine is when you want to create a deployment environment for your application and manage all the micro-services running on it
- To setup, all we need to do is point our `docker-machine` CLI at our managed host, which enables us to run docker commands directly on that host.

### Connecting to Docker Machine
The connection to a docker machine is made available through env variables. By default, they are unset, giving us our default connection
`docker-machine env -u` will show us how to unset all variables to return to our default connection
If we wanted to connect to minikube, we could run `eval $(minikube docker-env)
