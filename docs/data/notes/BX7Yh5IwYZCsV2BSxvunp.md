

SOA is kind of a precursor to microservices. Microservices solved a lot of issues that SOA had, such as their heavy weight.

SOA is well suited in an environment where services are provided between software components using a communication protocol (like HTTP).

Service orientation is a way of thinking in terms of services and the outcomes of those services.

There are 4 properties of a service:
1. It logically represents a repeatable business activity with a specified outcome
2. It is self contained.
3. Those who consume the service view it as a black box.
4. The service may be composed of other services.

### Service Mesh
A SOA architecture may employ a Service Mesh, which is a dedicated infrastructure layer for facilitating service-to-service communication.

The service mesh uses a proxy

The service mesh can provide some benefits, such as:
- provide observability into communications (ie. system monitoring)
- provide secure connections
- automating retries

A service mesh consists of network proxies paired with each service, along with a set of task management processes.
- proxies are known as the Data Plane.
	- Data plane intercepts calls between different services and processes them
- management processes known as Control Plane.
	- Control plane is the brain of the mesh that coordinates behaviour of proxies and provides APIs for operations personnel to manipulate and observe the entire network.

[Difference between SOA and microservices](https://www.ibm.com/cloud/blog/soa-vs-microservices)

# UE Resources
[Anti-patterns in SOA](https://www.infoq.com/articles/SOA-anti-patterns/)
