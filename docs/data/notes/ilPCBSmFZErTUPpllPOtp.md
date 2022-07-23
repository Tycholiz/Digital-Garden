
Instead of always trying to follow best practices, it's often a better idea to avoid *anti-patterns*.
- The term *best practices* seems to imply that there are clear cut paths set out for us, which is only true some of the time. Most of the time, there are exceptions in our specific use-case which throws a wrench in our plans to follow best practices.

### Fault tolerance
The best way of building fault-tolerant systems is to find some general-purpose abstractions with useful guarantees, implement them once, and then let applications rely on those guarantees.
- ex. because of the abstraction provided by [[transactions|db.strategies.transaction]], the application can pretend that there are no crashes (atomicity), that nobody else is concurrently accessing the database (isolation), and that storage devices are perfectly reliable (durability).

### Coupling
Couping is a much worse problem than non-DRY code.

### Rules
If you are going to make a rule about something, make sure to provide good error handling for the user. Put them in a position where they can get feedback about the rule as quickly as possible
- ex. if you make a rule about Git branch naming that "branches must not contain a `_` character", then you should set up the means to have the user warned any time they create a branch with naming like this.