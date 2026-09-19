
# Overview
Event emitters are objects that trigger an event by sending a message when an action occurs (therefore, this is [[pub/sub|general.patterns.messaging.pubsub]]).
- we can write code that listens to events from event emitters and execute some code in response.

The typical modern way in Javascript to perform some action after completion of another is with [[promises|js.lang.promises]].
- a downside of promises is that they tightly couple the triggering action to the resulting action, making it difficult to modify the resulting action in the future. This is a general benefit that is offered by Pub-Sub patterns over more traditional call-and-response approaches.
    - expl: If we wanted to change the behavior of our application, we could adjust how the subscribers react to the events without having to change the publisher.

the `emit()` function returns `true` if there are listeners for the event. If there are no listeners for an event, it returns `false`.

Sometimes we’re only interested in listening to the first time an event was fired, as opposed to all the times it’s emitted. Node.js provides an alternative to `on()` for this case with the `once()` function.
- when the event is emitted and received by a listener that uses `once()`, Node.js automatically removes the listener and then executes the code in the callback function.


## Creating Event Emitters
There are two common ways to create an event emitter in Node.js. 
1. use an event emitter object directly
    - Do this if the events you want to emit are independent of your business objects or are a result of actions from many business objects
2. create an object that extends the event emitter object
    - Do this if the events you want to emit are an effect of an object's actions, in order to have access to its functions for convenience.
        - ex. if we are making a `TicketManager` class, we could have a method `buy` which performs some database interaction, but then emits the `buy` event. This is a good use-case for extending the `EventEmitter`
    
```js
const EventEmitter = require("events");

class TicketManager extends EventEmitter {
    constructor(supply) {
        super();
        this.supply = supply;
    }
    
    buy(email, price) {
        this.supply--;
        this.emit("buy", email, price, Date.now());
    }
}
```

# UE Resources
[Event-emitters](https://www.digitalocean.com/community/tutorials/using-event-emitters-in-node-js)
