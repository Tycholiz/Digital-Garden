
Tight coupling has many causes:
- Mutation vs immutability
- Side-Effects vs purity/isolated side-effects
- Responsibility overload vs Do One Thing (DOT)
- Procedural instructions vs describing structure
- Class Inheritance vs composition

Imperative and object-oriented code is more susceptible to tight coupling than functional code.
- pure functions are less vulnerable to tight coupling by nature.

## Should this code be decoupled?
When considering whether or not code should be decoupled, you have to consider both modules and ask if there is a reasonable likelihood that one could exist without the other. It is easy to fall into the trap of only considering if one module should ever need to exist on its own, while completely ignoring the other one.

### Illustration of this thought
consider the case where we had considered adding the is_subscribed argument to the register_user function.

My first point of view was that "we should not include this as an argument in the function, because it couples the logic of registering a new account with the logic of subscribing that user to a mailing list. What if we want to register a user without having to decide about subscribing? Why is this logic coupled?"

This is how I first reasoned about it, but realistically, I should have looked at the other side of it too: "is there any place in the code that we'd want to set a user as subscribed, without registering them also?".

The lesson is: when considering questions of if code should be decoupled, look at all modules that are being decoupled together, and ask "will any of these modules need to be used without the other?"

* * *

Coupled code is not necessarily an anti-pattern. You have to ask if these modules should live together under all circumstances. Would it make sense for one to exist without the other? (again, thinking about it from both angles)
- ex. webpage data. We can introduce 3 separate hooks to the page, each which gets a different piece of data, or we can wrap these 3 hooks into a separate hook, which couples the 3 hooks together, and indirectly to the page (since this is a very specific set of data, we can now consider coupled to the concept of this particular page, which requires this particular set of data).
    - anal. Think of how a Cable Management Sleeve is used. We wrap it around multiple cables to "abstract away" the need to concern ourselves with each cable. We would only ever wrap cables that we are pretty sure would stay together. If we had only one charging cable for our laptop and had to carry it around wherever we go, then we wouldn't wrap this cable in the sleeve. This analogy helps shows that there are costs and benefits to each strategy.

A common pattern is that software starts out built in a decoupled way, but eventually becomes more decoupled over time.
- ex. consider 2 services: OrderService and InvoiceService. The OrderService gets called the a user signals intent to buy, and then it calls the InvoiceService. For all intents and purposes, these 2 are decoupled. However, with this architecture the OrderService has the burden of calling the downstream service's API. In the simple case where we have InvoiceService, that is no problem. However, once we add more, the burden gets heavier and heavier. Soon it becomes apparent that there is a tight coupling between OrderService and the other services that sit downstream from it.
