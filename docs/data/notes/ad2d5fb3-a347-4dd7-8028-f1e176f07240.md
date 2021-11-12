
# Networks
- Networks enable containers to be able to communicate with each other and with non-Docker processes (such as a host) 
- Networks are natural ways to isolate containers from other containers or other networks. As such, they provide complete isolation for containers
- Docker’s networking subsystem is pluggable, made possible by having drivers
	- depending on which driver you are using, you will have different core networking functionality 
- `docker network ls` - show all networks
- all containers within a network can communicate with each other. This can be shown by the fact that you can ping the IP address of one container from another (within the same network) 
	- you can also simply `ping <container-name>`
- Docker networking allows you to attach a container to as many networks as you like.
- to see if 2 containers are properly on the same network, try pinging one container's IP from another.

There are 2 main types of network: bridge and overlay

## Bridge
- this is the default
	- Docker creates a bridge named `docker0`, and both the docker host and the docker containers have an IP address on that bridge.
- Bridge networks are usually used when your applications run in standalone containers that need to communicate over the same docker host (ex. a pod?)
- Limited to a single host running Docker Engine.
- default type
- if our `docker-compose.yml` does not explicitly specify a network to use, a special network called *bridge* will be the network that our containers are launched in 
	- visible with `docker network ls`

## Overlay
- Overlay networks connect multiple Docker daemons together and enable swarm services to communicate with each other.
- can include multiple hosts and is a more advanced configuration

## Host network
- if we have a standalone container, network isolation between the container and the Docker host will be removed, and the container will use the host's network directly
- host is only available for swarm services
