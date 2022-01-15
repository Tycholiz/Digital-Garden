
# Orchestrators (container schedulers)
- Tools to manage, scale, and maintain containerized applications
- ie. Kubernetes, Docker Swarm
- Orchestration tools are designed to handle Docker containers running stateless applications
	- as such, you should not run stateful applications in orchestration tools which are built for stateless apps.
	- this makes sense, seeing as containers can be killed and restarted without any issue, multiple can run at once etc.
		- The same cannot be said for a database, which is why we typically don't dockerize DBs

The container scheduler is a framework that determines where a service should be deployed and make sure that it maintains the desired run-time specification. The scheduler manages clusters (which are composed of services)

A cluster scheduler has quite a few goals.
- It makes sure that resources are used efficiently and within constraints.
- It makes sure that services are (almost) always running.
- It provides fault tolerance and high availability.
- It makes sure that the specified number of replicas are deployed.
- It makes sure that the desired state requirement of a service or a node is (almost) always fulfilled. Instead of using imperative methods to achieve our goals, with schedulers, we can be declarative.
- We can tell a scheduler what the desired state is, and it will do its best to ensure that our desire is (almost) always fulfilled.
	- ex. instead of executing a deployment process five times hoping that we’ll have five replicas of a service, we can tell a scheduler that our desired state is to have the service running with five replicas.

Consider the impact that is had by having declarative methods, as is offered from a container scheduler. With a declarative expression of the desired state, a scheduler can monitor a cluster and perform actions whenever the actual state does not match the desired. Compare that to an execution of a deployment script. Both will deploy a service and produce the same initial result. However, the script will not make sure that the result is maintained over time. If an hour later, one of the replicas fail, our system will be compromised.
- traditionally, these problems were solved by having alerts, and having DevOps guys manually intervene to get everything back up and running.
- Think of schedulers as operators who are continually monitoring the system and fixing discrepancies between the desired and the actual state

In general, the development workflow looks like this:
1. Create and test individual containers for each component of your application by first creating Docker images.
2. Assemble your containers and supporting infrastructure into a complete application, expressed either as a Docker stack file or in Kubernetes YAML.
3. Test, share and deploy your complete containerized application.

* * *

You should not run stateful applications (like a db) in orchestration tools which are built for stateless apps.
- Orchestration tools are designed to handle Docker containers running stateless applications. Such applications don’t mind being terminated at any time, any number can run at the same time without communicating with each other and nobody will really notice if a new container will take over on a different machine.
