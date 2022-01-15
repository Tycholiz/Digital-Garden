
## Overview
Kubernetes is all about abstracting how, when and where containers are run.
 
Kubernetes is a [[docker orchestrator|docker.orchestrators]]. It can also be thought of as a container scheduler.
All containers in Kubernetes are scheduled as pods
- We don't tell Kubernetes to run a container. Rather, we tell it to create a pod that wraps a container.

Kubernetes uses client-server architecture

It’s not enough to run containers. We need to be able to scale them, to make them fault tolerant, to provide transparent communication across a cluster
- Containers are only a low-level piece of the puzzle. The real benefits are obtained with tools that sit on top of containers. Those tools are today known as container schedulers. They are our interface. We do not manage containers, they do.

Kubernetes follows the ethos that we should not run our containers directly, and should instead trust Kubernetes to handle container scheduling for us.
- The chief reasoning for this is that containers by themselves don't provide fault tolerance. They cannot be deployed easily to the optimum spot in a cluster, and are not operator friendly

Today, modern infrastructure is created from immutable images. Any upgrade is performed by building new images and performing rolling updates that will replace VMs one by one. Infrastructure dependencies are never changed at runtime
- One of the inherent benefits behind immutability is a clear division between infrastructure and deployments. Until not long ago, the two meshed together into an inseparable process. With infrastructure becoming a service, deployment processes can be clearly separated, thus allowing different teams, individuals, and expertise to take control.

spec: Kubernetes offers us process isolation (via Docker), and an immutable way to deploy software

### Benefits of Kubernetes
- We can use it to deploy our services, to roll out new releases without downtime, and to scale (or de-scale) those services.
- We can move a Kubernetes cluster from one hosting vendor to another without changing (almost) any of the deployment and management processes.

“A Kubernetes cluster is a good example of an abstraction over compute resources: there are many hosted and self-managed implementations of it on different platforms, all of which offer a common API and common set of capabilities.”
- That is, to a large extent, it doesn't matter if you are using AWS, Azure etc. Most of the config of K8s will be identical. Thus, Kubernetes is a good abstraction over compute as a resource.

Kubernetes is usually seen as a way to manage your production infrastructure. But it can be configured to manage our test environments on the fly. Imagine using Kubernetes to host a Preview Environment. Every pull request can be run in an isolated test environment at the push of a button.
- "isolated" here truly means isolated. the app itself (with the code changes in the PR branch) with its own dedicated instances of SQL Server, Redis, Elasticsearch, and additional services pieces. All spun up from scratch within minutes and running in a handful of containers in a dedicated namespace, just for you and anyone who’s interested in your PR.  
    - the benefit of this really shines in a [[microservice architecture|general.arch.microservice]]. In a monorepo, you just checkout someone's branch and run the code, but how do you do that if the code depends on many external services?

### Kubernetes vs Docker Swarm mode
In Kubernetes, an application can be deployed using a combination of pods, deployments, and services (or micro-services).

Whereas, in Docker Swarm, applications can be deployed as services (or micro-services) in a Swarm cluster. YAML files can be used to specify multi-container. Moreover, Docker Compose can deploy the app.

# E Resources
[Why to use Kubernetes from Day 1](https://stackoverflow.blog/2021/07/21/why-you-should-build-on-kubernetes-from-day-one/)
[Kubernetes Illustrated Children's Guide](https://chrisshort.net/kubernetes-illustrated-childrens-guide/)
[1 year using Kubernetes in production](https://techbeacon.com/devops/one-year-using-kubernetes-production-lessons-learned)
