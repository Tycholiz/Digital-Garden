
Instead of always trying to follow best practices, it's often a better idea to avoid *anti-patterns*.
- The term *best practices* seems to imply that there are clear cut paths set out for us, which is only true some of the time. Most of the time, there are exceptions in our specific use-case which throws a wrench in our plans to follow best practices.
- That being said, anti-patterns, like patterns, are about contextual wisdom. A discussion of anti-patterns needs to talk both about when a behavior is a problem and when that behavior might be acceptable.

### Fault tolerance
The best way of building fault-tolerant systems is to find some general-purpose abstractions with useful guarantees, implement them once, and then let applications rely on those guarantees.
- ex. because of the abstraction provided by [[transactions|db.strategies.transaction]], the application can pretend that there are no crashes (atomicity), that nobody else is concurrently accessing the database (isolation), and that storage devices are perfectly reliable (durability).

### Coupling
Signs that there is too much coupling:
- Changes in one part of the code cause unexpected changes in other parts
- High dependency on specific implementations (ie. a module or class has a lot of dependencies on other specific modules or classes)
- Difficulty in testing (ie. challenging to test a particular module or function in isolation)
- Difficulty in maintaining (challenging to modify or extend a module or function without making changes to many other parts of the codebase)
- Tight coupling between business logic and infrastructure: If the business logic of an application is too tightly coupled to infrastructure-related code (e.g., database access or network requests), it may make the code harder to maintain or to reuse in other contexts.
- Repeated code or functionality: If there is a lot of repeated code or functionality throughout the codebase, it can be a sign of high coupling.

Coupling is a much worse problem than non-DRY code.
- This is because too much coupling can lead to a codebase that is difficult to modify and maintain, making it harder to add new features or fix bugs. non-DRY code can still be relatively easy to maintain if it is organized well and follows good coding practices
- Non-dry code is also much easier to refactor than code that is overly coupled.

see [[Dendron note on decoupling|general.patterns.decoupling]]

### Rules
If you are going to make a rule about something, make sure to provide good error handling for the user. Put them in a position where they can get feedback about the rule as quickly as possible. The more seemingly arbitrary the rule, the more important this practice becomes.
- ex. if you make a rule about Git branch naming that "branches must not contain a `_` character", then you should set up the means to have the user warned any time they create a branch with naming like this.