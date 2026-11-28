
An API gateway acts as a [[reverse proxy|webserver.proxy.reverse]] to accept all API calls, aggregate the various services required to fulfill them, and return the appropriate result.

API gateways can be used to unify access control across all the APIs that sit behind
- ex. authentication, rate limiting, and analytics

Applications that are built using [[microservices|general.arch.microservice]] most likely use an API gateway, since a single API call could result in dozens of calls to distinct APIs

Having an API gateway allows querying users to continue to access the API in the same fashion indefinitely, even if APIs behind the gateway are modified.

An API Gateway will use a [Circuit Breaker](https://microservices.io/patterns/reliability/circuit-breaker.html) to invoke services

In implementing an API gateway, an event-driven/reactive approach is best if it must scale to scale to handle high loads.
- AWS offers [[AWS API Gateway|aws.svc.api-gateway]]

### Downsides
- Increased complexity - the API gateway is yet another moving part that must be developed, deployed and managed
- Increased response time due to the additional network hop through the API gateway - however, for most applications the cost of an extra roundtrip is insignificant.

* * * 

### Backends for Frontends
The *Backends for frontends* pattern is a variation on the API gateway pattern

This pattern defines a separate API gateway for each kind of client
- ex. API gateways for web application, mobile application etc.

![](/assets/images/2023-02-06-12-10-18.png)

### API Composition
In systems that apply both the Microservices pattern and the *Database per service* pattern, it is no longer straightforward to implement queries that join data from multiple services.
- this is the role of an *API Composer*, which invokes the services that own the data and performs an in-memory join of the results.
- [[general.patterns.architectural.CQRS]] is an alternative approach to this problem


## UE Resources
- [Netflix API gateway](https://netflixtechblog.com/optimizing-the-netflix-api-5c9ac715cf19)