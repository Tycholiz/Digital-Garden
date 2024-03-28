
Implementations exist at a given level of abstraction. If the term abstraction in essence refers to how low-level or high-level our context headspace is, then the tools we have at our disposal each operate at a certain level of abstraction.
- [[redux]] in my React app operates at a certain level of abstraction, and the [[os.kernel]] in my OS operates at a totally different one, but in essence, the Redux application depends on the kernel doing its job properly.

The root of all abstractions is electricity. From that, a [[stream|general.patterns.streaming]] of 0s and 1s are produced, and so forth. Developers design and build programs that operate at a certain level of abstraction.
- ex. wires exist at a level of abstraction as much as an [[os.process]] does. When we think about it, wires are technology that has been developed to serve a purpose; no different than any other implementation (or outcome) of code.

### Leaky Abstraction
A leaky abstraction is one that doesn't provide a clean and complete separation between higher-level and lower-level systems, causing developers to deal with the underlying complexities when they should ideally be shielded from them.

A leaky abstraction occurs when we have built some abstraction, but some of the implementation details of the lower-level things we were supposed to have abstracted leaked through the API and are made available to the user of the abstraction.
- the problem with this is that it needlessly makes the abstraction more complex
- also, if those lower-level implementation details are modifiable, it makes the abstraction brittle, since changes to the lower-level details can break modules that depend on it.

If an abstraction is leaky, it means that the consumer of that API must know *exactly* how it works in order to avoid pitfalls.

#### Example: Abstraction to hide SQL-specific implementations
Imagine we had a module that abstracted away details about whether the user was using MySQL, Postgres or any other SQL engine. Ideally, the user of the abstraction doesn't have to be aware of which engine they are using. We would have a leaky abstraction if the user had to eschew this pretense and do something "the MySQL way" in order to solve a problem. 