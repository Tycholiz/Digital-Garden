
- Traditionally, we use only databases as sole data storage, and think of that data in terms of "things", along with their state.
    - Kafka encourages us to think of events first, and things second.
- Kafka stores data in a distributed log instead of a database.
    - log is an ordered sequence of events, along with state and a description of what happened
    - Kafka is a system for managing these logs (in Kafka, a log is called a `Topic`)
- logs are easy to build at scale
- Implementations of Kafka are declarative
- Kafka can be quite complex to operate.
    - AWS offers MSK (Managed Streaming for Kafka), making the management of a Kafka cluster trivial.

### Topic
An ordered collection of events stored in a durable way.
spec: A topic represents a single type of event
- Durable - written to disk, and replicated
- Can be thought of as a *real-time stream*
- Topics can be thought of as a database table
    - ex. if we were doing this in Postgres, we could have tables `geofence_entry_events`, `low_tire_pressure_warning_events`

Kafka Services can be used as a sort of glue between microservices of an application. A microservice can consume a message from the Kafka Topic, and produce an output which gets registered to another Topic.
- Since it can act as glue between many services, we can produce an output from these Topics that can be consumed by a new service to perform real-time analysis of that data
    - This is contrast to the old-school method of running a batch-process overnight

### Kafka Connect
Imagine we have multiple databases, a legacy service, and a SaaS product, and we want a way to get the data that they produce into Kafka.
- Kafka Connect helps us get that data into Kafka, and back out again.
- Kafka Connect is a general term to refer to 100's of pluggable modules that handle the I/O of the whatever service we are connecting to.
    - ex. There would be a connector to capture row-level changes in a Postgres database.

Connectors are either Source or Sink Connectors, and are responsible for a some of the Task management, but not the actual data movement.

### Kafka Streams
- A Java API that performs helps us perform grouping, aggregating, filtering, enrichment (table joining)
- the API would be used from within the services
- This is available to us out of the box as a consequence of using Kafka.

#### KSQL
- a language that allows us to to use SQL-like syntax to query data from one Topic, and output it into another Topic.
- Solves the problem statement: imagine we want to perform some analysis on data kept in Kafka, but we don't want to stand up a separate service to consume that data.

### Confluent
- a distribution of Kafka.
- open source, but offers a paid managed service (similar to Docker)

### Kafka vs Logstash
[Kafka is a cluster; Logstash is a single instance](https://stackoverflow.com/questions/40864312/how-logstash-is-different-than-kafka)

### Kafka vs RabbitMQ
These are different forms of communicating. When service A calls service B to exchange information, it's similar to a phone conversation. I ask you a question, and you respond. In the meantime we're both occupied with that conversation; you have to have time to talk with me, and so do I.

RabbitMQ and Kafka (and similar tools) also support two more types of communication:
1. *Topics*: a topic is a one-way one to many conversation. Basically it's like standing on a soapbox and shouting to a megaphone. Whoever is interested can listen; I don't care if that is zero people or a hundred. I send my message and the broker (kafka/rabbit) makes sure whoever is subscribed to the topic all get the message.
2. *Queues*: a queue is similar to sending letters to a company. I send a letter complaining about their service, and within that company someone opens and reads this letter. I'm not involved anymore after I send  the letter. While there may be a hundred workers opening letters, only one of them is going to be handling mine. Copies don't magically appear.

So both queues and topics are one-way communication that are fire and forget (asynchronous). This as opposed to TCP (and HTTP on top of that) that are two-way synchronous communication. The broker guarantees that the other side gets the message. Depending on how you configure (for example) Kafka; you can either have a single service get a message (queue example), all of the services get the same message (topic example) or something in between.

When working with microservices you want every unit to be as decoupled as possible.
- Let's say you have a component that handles user information (`A`) and you have some other module that need to do something every time a user updates his info (`B`). The classic way to do it would be to make `A` send some signal (usually an http request) to `B` so it knows it has work to do. But, for that, `A` needs to know a lot about `B`. And what if you now have `C` that needs to be triggered to? Or `D` that needs to process just some of the users? `A` would need to keep track of all of this, meanings they are highly coupled.
- With a broker, `A` just publishes that there's a change, and all other components can subscribe to this event and react accordingly. This way, components can be added or removed with minimal impact on the overall structure.

## Do you need Kafka?
From Reddit [thread](https://www.reddit.com/r/apachekafka/comments/hyxezo/kafka_when_to_use_and_when_not_to_use/):

Kafka is like an event bus for distributed messages. It can't solve the intractable problem of distributed systems, but it does provide a nice framework for handling messages at scale.

I would say if you don't need Kafka, then that's a perfectly good reason not to use it. You might have a monolith, you might have a series of services that all own their data within a database, and they all communicate via REST APIs. If that works, then Kafka won't really serve a purpose.

You start to need Kafka when you run into the constraints of distributed scale. If you have more events than any single worker can handle, and you've decided you can't increase the available CPU and memory any further, then you start facing distributed messaging problems.

As soon as two computers located some distance from each other try to determine what is true, and what happened first, you'll run into problems. Your next goal is to try and figure out which constraints you're willing to bend.

Maybe your pipeline doesn't actually care about order, but absolutely cares that you don't drop a single record. Maybe you only care about throughput. Maybe you care about order, but not about availability. Eventually somethings gotta give. Kafka just helps you manage that infrastructure, it doesn't solve the underlying issues.

If you run an app that collects logs and metrics, order might not matter. Just send everything down the pipe and you'll aggregate later. But you probably wouldn't run your banking transaction on it.

### Using Kafka in a Logging system
"There are plenty of open-source tools available for logging. We decided to use Graylog—an excellent tool for logging—and Apache Kafka, a messaging system to collect and digest logs from our containers. The containers send logs to Kafka, and Kafka hands them off to Graylog for indexing. We chose to make the application components send logs to Kafka themselves so that we could stream logs in an easy-to-index format. Alternatively, there are tools that retrieve logs from outside the container and forward them to a logging solution."

# UE Resources
[The place to start learning Kafka](https://kafka-tutorials.confluent.io/)
[Getting Started](https://www.confluent.io/blog/getting-started-with-kafkajs/)
[Kafka with Azure Functions](https://github.com/Azure/azure-functions-kafka-extension)
[Connect Architecture](https://medium.com/@Instaclustr/apache-kafka-connect-architecture-overview-842097d3eb96)
