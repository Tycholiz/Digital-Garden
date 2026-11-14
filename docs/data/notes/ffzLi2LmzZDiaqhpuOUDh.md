
A rules engine can be visualized as a set of if-then-else statements to implement some business logic of our application.

Example: determining car insurance premiums
```
if car.owner.hasCellPhone then premium += 100;
if car.model.theftRating > 4 then premium += 200;
if car.owner.livesInDodgyArea && car.model.theftRating > 2 then premium += 300;
```

A rules engine is a tool that makes it easier to program using this computational model

An important property of rule engines is chaining - where the action part of one rule changes the state of the system in such a way that it alters the value of the condition part of other rules.
- Chaining sounds appealing, since it supports more complex behaviors, but can easily end up being very hard to reason about and debug.
	- Often, it is easy to set up a rules system, but very hard to maintain it because nobody can understand this implicit program flow; a consequence of leaving the imperative computational model

# E Resources
- [Martin Fowler thoughts on why to avoid a rules engine](https://martinfowler.com/bliki/RulesEngine.html)
- [JSON rules engine repo](https://github.com/cachecontrol/json-rules-engine)
