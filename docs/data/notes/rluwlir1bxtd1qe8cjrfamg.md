
In Nestjs, a microservice is an application that uses a transport layer that is not HTTP (such as TCP, which is the default)
- the specific transport layer implementation that is used is called a *transporter*.
- most transporters support both request-response and event-based message styles.

A Nestjs application can exchange messages or publish events to a Nest microservice using the `ClientProxy` class
- this class notably provides the `send()` and `emit()` methods, allowing us to communicate with remote microservices.
  - `send()` returns a cold [[general.patterns.behavioural.observable]] to which we can subscribe.
    - *cold* meaning we have to explicitly subscribe to it before the message will be sent
- this class is lazy; it only initiates a connection just prior to when the first microservice call is made.
- this class can be instantiated by either:
  - importing `ClientsModule` and registering it into one of your [[nestjs.modules]] as part of the `imports` array. This enables us to (dependency) inject an instance of `ClientProxy`:
  ```js
  constructor(
    @Inject('NAME_OF_SERVICE_AS_REGISTERED_IN_MODULE') private client: ClientProxy,
  ) {}
  ```
  - using the `@Client()` decorator, which is the non-preferred way

## Handlers
Only to exist within `@Controller()` decorated classes, since they are the entry points for your application

### `@MessagePattern()`
Decorator to create a message handler based on the request-response paradigm
- ex. Here, the `accumulate()` message handler listens for messages that fulfill the `{ cmd: 'sum' }` message pattern. The handler takes in a single argument `data`, which is passed from the client.
```ts
@MessagePattern({ cmd: 'sum' })
accumulate(data: number[]): number {
  return (data || []).reduce((a, b) => a + b);
}
```

Message handlers support both sync and `async`

A message handler is able to return an [[observable|general.patterns.behavioural.observable]].
- in this case, the result values will be emitted until the stream is completed.

There is a bit of overhead involved in this paradigm, since the Nestjs runtime creates 2 channels to handle the request and response.

### `@EventPattern()`
Decorator to create an event handler based on the event-based paradigm (ie. when you want to publish events without waiting for a response).

Ideal use-case is if you would like to simply notify another service that a certain condition has occurred in this part of the system.

You can register multiple event handlers for a single event pattern (ie. the decorator arg) and all of them will be automatically triggered in parallel.

If using Kafka, you may want to access the message headers. In order to accomplish that, you can use built-in decorators as follows
- note: types are obtained from `@nestjs/microservices` library

```ts
@EventPattern(env.KAFKA_TOPIC)
async stateChangeHandler(@Payload() event: Record<string, unknown>, @Ctx() context: KafkaContext) {
  const label = 'AppController#stateChangeRivian';
  await this.allTopics(event, context.getTopic(), label);
}
```
- note: You can also pass in a property key to the `@Payload()` decorator to extract a specific property from the incoming payload object. 
  - ex. `@Payload('id')`