
An event can be defined as "a significant change in state".
- ex. when a consumer purchases a car, the car's state changes from "for sale" to "sold"

spec: An event can be described as any change in state that can be measured in terms of `true`/`false`.

Events do not travel, they just occur.

Event-driven architecture can complement [[SOA|general.arch.SOA]] because services can be activated by triggers fired on incoming events

## Illustration of the problem it solves
Consider if we had an API for handling an order. First, the customer signals their intent to make a purchase, then an invoice is generated. We have these as two decoupled systems. At least, that's what they appear to be:

![](/assets/images/2021-11-22-15-02-44.png)

But what happens when you start to create more services downstream from the Order Service?

![](/assets/images/2021-11-22-15-14-23.png)

Here, each downstream service must publish its own API, and the Order Service becomes responsible for talking to each of them. To accomplish this, the Order Service must understand retry semantics for each service, as well as bundle an SDK for each. As more downstream services are introduced, they become dependent on the Order Service, introducing an integration cost to hooking up to the Order Service.

This architecture also has a tendency to get complicated easily. Imagine the FulfillmentService doesn't have the item in stock, and therefore has to inform the InvoiceService? Then we have to choreograph the flow of all of this business logic.

Now, imagine that someone created a RewardsService to track consumer reward points, but your team wasn't informed. Now the RewardService is in a position where it doesn't know what's happened.

## Overview
Events are observable, not directed.
- In the Payment Domain example, the Service APIs used direct commands, whereas the event-driven approach is observable.

![](/assets/images/2021-11-22-15-21-57.png)

The benefit of the event-driven approach is that the services emitting the events don't have to have any knowledge about who is listening to those events. That burden is removed from the emitter and placed onto the consuming service.
- The end result is that service can consume upstream events without requiring upstream changes.

The event producer sends events to an event bus via an endpoint. A router manages directing and filtering those events to the appropriate downstream consumers.

Now, we have an architecture where the OrderService sends events to the Event bus, which gets configured to recognize which downstream services should be notified of the event.
![](/assets/images/2021-11-22-15-26-31.png)

Another benefit of this architecture is that if OrderService throws an error, it can send an Error event to the event bus, and all the subscribed nodes will be notified of it so they can act accordingly.

In the above example, we were at a disadvantage when one team added a RewardService without telling the OrderService team. In this Event-driven architecture, the RewardService team only needs to add a new rule to the bus to be subscribed to emitted events from OrderService.
