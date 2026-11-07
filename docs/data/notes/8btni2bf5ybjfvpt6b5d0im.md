
If an application server connects to a database 100 times per second and the database fails, the same error will be thrown to the application server over and over. Furthermore, the application server will have to wait for the TCP connection to timeout before receiving that error.

By implementing the circuit breaker pattern, the application server can check the availability of that database. After implementing this pattern, the circuit breaker will detect failures and will prevent the application from trying to perform an action that is doomed to fail.

The circuit breaker works by recording the state of the external service on a set interval

## Why use it?
- *Fault Isolation*: The Circuit Breaker isolates the impact of a failing service, preventing the failure from propagating to other parts of the system and causing a cascading effect.
- *Fast Failure Detection*: The Circuit Breaker quickly detects and responds to failures, reducing the response time and preventing the system from wasting resources on calls to a failing service.
- *Graceful Degradation*: By providing fallback behavior, the Circuit Breaker enables the system to gracefully degrade in the face of failures, ensuring that some level of functionality is available to clients even when the primary service is unavailable.
- *Automatic Recovery*: The Circuit Breaker provides an automatic recovery mechanism. After the service has stabilized, it automatically allows requests to pass through again, restoring normal operation.

## States
The circuit breaker implements a [[general.patterns.state-machine]] that can exist in 3 different states: closed, open or half-open

### Closed
This is the happy path state, and all requests to the service will pass through without incident

### Open
This state is enabled once the number of failures increases beyond the configured threshold

In this state circuit breaker returns an error immediately without even invoking the services

### Half-open
This state is enabled after a set period of time elapses of being in the Open state

In this state, the circuit breaker allows a limited number of requests from the Microservice to passthrough and invoke the operation.
- If the requests are successful, then the circuit breaker will go to the closed state. 
- If the requests continue to fail, then it goes back to Open state.