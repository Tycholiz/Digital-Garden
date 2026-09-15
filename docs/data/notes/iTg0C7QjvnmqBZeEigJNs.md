
## Overview
Opentracing is a vendor-agnostic API to achieve distributed tracing in a microservice architecture.

Distributed tracing tracks a single request through all of its journey, from its source to its destination. This means if we have a frontend that makes a request to a server, and that server then fires off a stored procedure from a database, then returns to the server, and triggers some other events to happen, this will all appear on the trace.
- normal traces will only track a request through a single application domain.
- Therefore, we can say that distributed tracing is the stitching of multiple requests across multiple systems. The stitching is often done by one or more `correlationId`s, and the tracing is often a set of recorded, structured log events across all the systems, stored in a central place.

In OpenTracing, a trace is a directed acyclic graph of Spans with References that may look like this:
```
[Span A]  ←←←(the root span)
            |
     +------+------+
     |             |
 [Span B]      [Span C] ←←←(Span C is a `ChildOf` Span A)
     |             |
 [Span D]      +---+-------+
               |           |
           [Span E]    [Span F] >>> [Span G] >>> [Span H]
                                       ↑
                                       ↑
                                       ↑
                         (Span G `FollowsFrom` Span F)
```

This allows us to model how our application calls out to other applications, internal functions, asynchronous jobs, etc. All of these can be modeled as Spans

### Span
![[opentracing.span]]

## Tips for Implementation
- Use dependency injection where possible. This will make things easily testable and configurable.
- Follow the idioms of your language and frameworks as much as possible. This will let your team members easily onboard into OpenTracing and tracing in general.
- Many frameworks provide extensibility points around units of work. For example, Spring Boot has pre- and post-request handlers for web requests. Leverage these as much as possible to save you effort when instrumenting tracing.
- If you don’t have a framework or can’t use its extensibility points, keep most of the tracing instrumentation as isolated from the business logic as possible. You can use patterns like Decorator and Chain of Responsibility for this, or even aspect-oriented programming. This will make your code’s intent clearer and easier to read. Exceptions to this include when you need to add tags or log an event; these are commonly specific to the business logic your code is executing.

## E Resources
- [Sentry primer on tracing](https://docs.sentry.io/product/sentry-basics/tracing/distributed-tracing/)