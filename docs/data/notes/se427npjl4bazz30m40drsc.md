
Kafka Streams is a stream-processing (Java-based) API that gives us access to all of the computational primitives of stream processing (filtering, grouping, aggregating, joining etc.)
- Using Streams negates our need to write framework code on top of our consumer API to do these things
- Using Streams also provides direct support for the potentially large amounts of state that result from doing stream processing operations, negating our need to have to manage that state (and thereby negating our need to have to worry about fault-tolerance)
    - this is notable, since just about anything interesting we are going to be doing in a consumer is going to be stateful (e.g. grouping events by a field with many unique values, then performing an aggregation over that group every hour).

A Kafka Streams application is a consumer group, meaning it has its own scaling capabilities built in.

Kafka Streams is a library rather, than an infrastructure
- that is, it's something you add to an application

Streams persists the state to internal topics in the Kafka cluster, allowing us to restore state in the event of failure.
- this helps not only in event of failure, but also when we're adding/removing stream processing nodes to our consumer group.

Over time, the consumer-side of a Kafka system tends to grow in complexity (though not so much producers)

https://kafka.apache.org/documentation/streams/