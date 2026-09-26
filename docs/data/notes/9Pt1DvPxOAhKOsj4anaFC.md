
The “span” is the primary building block of a distributed trace, representing an individual unit of work done in a distributed system.
- Each component of the distributed system contributes a span- a named, timed operation representing a piece of the workflow.

A Span represents a separate software system communicating over messaging or HTTP.

Spans can (and generally do) contain “References” to other spans, which allows multiple Spans to be assembled into one complete Trace - a visualization of the life of a request as it moves through a distributed system.

Each span encapsulates the following state according to the OpenTracing specification:
- An operation name
- A start timestamp and finish timestamp
- A set of key:value span Tags
- A set of key:value span Logs
- A SpanContext

Spans can connect to each other via two types of relationship: ChildOf and FollowsFrom. ChildOf Spans are spans like in our previous example, where our ordering website sent child requests to both our payment system and inventory system. FollowsFrom Spans are just a chain of sequential Spans. So, a FollowsFrom Span is just saying, “I started after this other Span.”

## Parts of a Span
### Tag
![[opentracing.span.tag]]

### Log
![[opentracing.span.log]]

### SpanContext
Carries data across process boundaries.

It has two major components:
1. An implementation-dependent state to refer to the distinct span within a trace
    - i.e., the implementing Tracer’s definition of spanID and traceID
2. Any **Baggage Items**
    - These are key:value pairs that cross process-boundaries.
    - These may be useful to have some data available for access throughout the trace.

#### Example Span:
```
    t=0            operation name: db_query               t=x

     +-----------------------------------------------------+
     | · · · · · · · · · ·    Span     · · · · · · · · · · |
     +-----------------------------------------------------+

Tags:
- db.instance:"customers"
- db.statement:"SELECT * FROM mytable WHERE foo='bar'"
- peer.address:"mysql://127.0.0.1:3306/customers"

Logs:
- message:"Can't connect to mysql server on '127.0.0.1'(10061)"

SpanContext:
- trace_id:"abc123"
- span_id:"xyz789"
- Baggage Items:
  - special_id:"vsid1738"
```
