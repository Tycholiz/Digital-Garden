
A microservice architecture is variant of the [[SOA|general.arch.SOA]] structural style
- arranges an application as a collection of loosely-coupled services.
- services are fine-grained and the protocols are lightweight.

goal is that teams can bring their services life independent of others, but it comes at a cost to maintain the decoupling.
- Interfaces need to be designed carefully and treated as a public API.
	- These difficulties are usually solved by either having versioned APIs, or multiple interfaces on the same service (ie. multiple commands that effectively export a document— just done in different ways)

A microservice is a self-contained piece of business functionality with clear interfaces
- through its own internal components, it may implement a layered architecture

The architecture essentially follows the Unix philosophy of "Do one thing and do it well"

It lends itself to a continuous delivery software development process.
- A change to a small part of the application only requires rebuilding and redeploying only one or a small number of services

Adheres to principles such as fine-grained interfaces (to independently deployable services), business-driven development (e.g. domain-driven design).

Microservice architectures are commonly adopted for cloud-native applications, serverless computing, and applications using lightweight container deployment.

When implementing a microservice-based architecture, the most important technology choices are the way microservices communicate with each other (synchronous, asynchronous, UI integration) and the protocols used for the communication (RESTful HTTP, messaging, GraphQL ...). Things like language and infrastructure don't introduce the coupling between services as you'd find in a monolithic app.
- In a traditional architecture, the choice of language impacts the whole system. This isn't the case in microservices,

## Pros & Cons
### Pros
Modularity
- This makes the application easier to understand, develop, test, and become more resilient to architecture erosion. This benefit is often argued in comparison to the complexity of monolithic architectures.

Scalability
- Since microservices are implemented and deployed independently of each other, i.e. they run within independent processes, they can be monitored and scaled independently.

Integration of heterogeneous and legacy systems
- microservices is considered as a viable means for modernizing existing monolithic software application. There are experience reports of several companies who have successfully replaced (parts of) their existing software by microservices, or are in the process of doing so. The process for Software modernization of legacy applications is done using an incremental approach.

Distributed development
- it parallelizes development by enabling small autonomous teams to develop, deploy and scale their respective services independently. It also allows the architecture of an individual service to emerge through continuous refactoring. Microservice-based architectures facilitate continuous integration, continuous delivery and deployment.
	- teams working on monoliths need to synchronize to deploy together.

### Cons
- Services form information barriers.
- Inter-service calls over a network have a higher cost in terms of network latency
- Testing and deployment are more complicated
- Moving responsibilities between services is more difficult. It may involve communication between different teams, depending on how messages are passed between each microservice.
- following this paradigm results in a tendency to see the microservice as the atomic unit. This can lead to too many services when the alternative of internal modularization of a more monolithic approach may lead to a simpler design.
	- To make this judgement requires understanding the overall architecture of the applications and interdependencies between components.
- The architecture introduces additional complexity and new problems to deal with, such as network latency, message format design, Backup/Availability/Consistency (BAC), load balancing and fault tolerance
mat design, Backup/Availability/Consistency (BAC), load balancing and fault tolerance

## Examples
### Amazon Product Page
Consider that a product page on Amazon gets data from multiple distinct domains:
- Product Info Service - basic metadata about the product such as title, author, page count etc.
- Pricing Service - product price
- Order service - purchase history for product
- Inventory service - product availability
- Review service - customer reviews 

The question becomes, how do the clients access each individual service? This is more complex than what we'd have in a monorepo architecture. As such, problems are:
- each client (web, mobile) might not necessarily get the same sets of data (ex. mobile probably shows less information)

# Resources
[List of Microservice Patterns](https://microservices.io/patterns/index.html)
