
*Event processing* involves tracking and processing streams of data about things that happen (ie. events) and deriving a conclusion from them.

*Complex Event Processing* involves gleaning useful information from the event streams as they arrive.
- the idea is to identify meaningful events
  - here, "meaningful" means as it relates to the business logic of an application
- these events may be originating from various layers of an organization, such as sales leads, orders or customer service calls. Or, they may be news items, text messages, social media posts, stock market feeds, traffic reports, weather reports etc.

CEP relies on a number of techniques, including:
- Pattern recognition of the events
- Event abstraction
- Event filtering
- Event aggregation and transformation
- Modeling event hierarchies
- Detecting relationships (such as causality, membership or timing) between events
- Abstracting event-driven processes

### Example: Church Monitor
Imagine we have a monitoring system in a church. One day, the system receives the following 3 events at around the same time:
1. church bells ringing.
2. the appearance of a man in a tuxedo with a woman in a flowing white gown.
3. rice flying through the air.

CEP as a technique helps discover complex events by analyzing and correlating other events
- From these events the monitoring system may infer a complex event: a wedding