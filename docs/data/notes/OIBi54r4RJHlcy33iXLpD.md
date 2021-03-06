
# Overview
Dependency Injection is one form of implementing [[Inversion of Control|general.patterns.IOC]]
- Here, IoC would be achieved through composition

A big reason to use dependency injection is to make your code more testable.

### Goal
The goal is to achieve [[separation of concerns|general.principles.SoC]]

The client should have no concrete knowledge of the specific implementation of its dependencies. It should only know the interface's name and API. As a result, the client will not need to change even if what is behind the interface changes. Because we achieve this by using interfaces, if the interface itself changes, then the client will have to change as well.
- ex. if the interface is refactored from being a class to an interface type (or vice versa) the client will need to be reworked.
- This last point creates a potential issue if the client and services are published separately.

### Motivation
Dependency injection separates the creation of a client's dependencies from the client's behavior, which allows program designs to be loosely coupled and to follow the dependency inversion and single responsibility principles.

A client who wants to call some services should not have to know how to construct those services. Instead, the client delegates the responsibility of providing its services to external code (the injector; see below)
- The client is not allowed to call the injector code; it is the injector that constructs the services

The injector then injects (passes) the services into the client which might already exist or may also be constructed by the injector. The client then uses the services. This means the client does not need to know about the injector, how to construct the services, or even which actual services it is using. The client only needs to know about the intrinsic interfaces of the services because these define how the client may use the services. This separates the responsibility of "use" from the responsibility of "construction".

### Components
Dependency injection involves four roles:
1. the service object(s) to be used
2. the client object that is depending on the service(s) it uses
3. the interfaces that define how the client may use the services
4. the injector, which is responsible for constructing the services and injecting them into the client
    - The injector may be referred to by other names such as: assembler, provider, container, factory, builder, spring, construction code, or main

As an analogy,
1. `service` - an electric, gas, hybrid, or diesel car
2. `client` - a driver who uses the car the same way regardless of the engine
3. `interface` - automatic, ensures driver does not have to understand details of shifting gears
4. `injector` - the parent who bought the car for the driver and decided which kind

Any object that may be used can be considered a service. Any object that uses other objects can be considered a client. The names have nothing to do with what the objects are for and everything to do with the role the objects play in any one injection.

## Pros and Cons
### Pros
- The client becomes an entity whose behavior is fixed, yet configurable. The client may act on anything that supports the intrinsic interface the client expects
- Because clients are more independent, they are easier to unit test. They are more like proper modules, that we can test in isolation, using stubs and mocks to simulate that module interacting with other modules.
- DI can be an effective way to refactor legacy code, because it does not require any change in code behavior.

### Explanation of terminology
An "Injection" is the basic unit of dependency injection, and it works in the same way that "parameter passing" works
- Referring to "parameter passing" as an injection carries the added implication that it is being done to isolate the client from details
- An injection doesn't care about how the passing is accomplished. For instance, it doesn't care if it is passed by value or by reference.
- An injection is about what is in control of the passing (never the client)

The DI pattern involves an object receiving other objects that it depends on.
- These "other objects" are called dependencies

Consider the client-server relationship.
- Here, the client is the object that receives, and the server is the object that is passed (ie. "injected")
- The code that passes the service to the client is called the *Injector*.
- Instead of the client specifying which service it will use, the injector tells the client what service to use. The "injection" refers to the passing of a dependency (a service) into the object (a client) that would use it.

## Examples

### Desktop/Laptop Analogy
Without DI: You have a laptop computer and you accidentally break the screen. And darn, you find the same model laptop screen is nowhere in the market. So you're stuck.

With DI: You have a desktop computer and you accidentally break the screen. You find you can just grab almost any desktop monitor from the market, and it works well with your desktop.

Your desktop successfully implements DI in this case. It accepts a variety type of monitors, while the laptop does not, it needs a specific screen to get fixed.

# UE Resources
[Expalantion with Nodejs](https://stackoverflow.com/questions/9250851/do-i-need-dependency-injection-in-nodejs-or-how-to-deal-with)
[DI with Node](https://khalilstemmler.com/articles/tutorials/dependency-injection-inversion-explained/)
[DI in Javascript with Sam](https://sammeechward.com/dependency-injection-in-javascript/)
