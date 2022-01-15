
Domain Driven Design is the concept that we should design our application code with heavy consideration into the business logic.
- ex. we don't know if it will always be the case that you must be a user to purchase a book. Therefore, we shouldn't design the system in a way where we can't accomplish that easily.

DDD is about thinking about your software in terms of business application, instead of looking at it in terms of technology.
- ie. we are writing for the needs of the business
- we achieve this by defining models for our domain, and then writing software that conforms to it.

DDD has nothing to do with writing code in a certain way. Rather, it's about understanding the underlying logic behind the problems you need to solve. It's the thinking about domains that drives the design of the code. In a way, DDD is the opposite of jumping right in with code and trying to solve something without thinking about it beforehand.
- "The novice jumps in right away with code. The veteran ponders the problem they are solving, and thinks (in code) about the logic of that problem"

DDD is all about unifying all of the code in a way that is highly comprehensible to those uninvolved with the code's creation. That is, focus is placed on the high level business logic and domain delineations.
- At a trivial level, it’s all about the names you use for things.  
- at the level above that, it’s about the way that you combine and activate things to produce business value.  
- at the level above that, it’s the causal and relationship (semantic) model that keeps everything cohesive, coherent, and aligned with the business.  
- this alignment can’t come from an implementation-focused model. you must use a domain model.

Why is this important?
- Centralizes (business) domain logic into the software to account for changes in understanding and business needs and to improve longevity 
- Helps us understand where we should be investing (strategic) and how to build it (tactical) - investing in the parts of the business that are core 
- Understanding the problem helps align everyone to produce better solutions and more effective communication

Key ideas:
1. Collaboratively model the domain so that your software reads like the problem it solves.
2. Isolate your software and establish context in each part of it. There should be enough previously handled assertions that you have a good idea of what's going to happen while looking at any random line of code. (Not sure how to word this one)
3. You don't have to model the domain *exactly*. 
    (Use abstraction to get rid of unnecessary details, and sometimes it makes sense to model things as they are used rather than what they actually are)

### Domain Primitives
The building blocks of DDD are [[value objects|general.objects.value-object]].
- They can also be thought of as *domain primitives*.
```ts
// without domain primitives
class User {
    name: string
    email: string
    mobile: string
}

// with domain primitives
class User {
    name: Name
    email: Email
    mobile: PhoneNumber
}

```

### Domain Model
a domain model is an abstraction of the subdomain. Not every aspect of the domain can be part of the model. It's the aspects chosen for implementation that constitute the model.
- this means that we don't just think in terms of what the object can do; we think of it in terms of what we *want* it to do.

### Farmyard Domain analogy
Imagine we wanted to build a house. We would first need to determine what type of house we are building. Detached? Condo? Duplex? Farmhouse?

After determining this, we would talk to the domain expert, which would probably be a farmer. 
- Notably, it's not the architect, since they wouldn't have the in-depth domain knowledge required to design everything.
![](/assets/images/2022-01-10-09-34-42.png)

- Domain: Farmyard
- Sub-domains: Barn, Stables, Farmhouse, Garden gate, etc.
- Domain model: The layout/plans for each sub-domain (e.g. the layout of the farmhouse)
    - analogous relationship:
        - classes -> objects
        - domain model -> bounded contexts
- Ubiquitous language: in the layout, we have rooms like Family room, Bedroom, Bathroom. This is the ubiquitous language of the domain.
- [[Bounded context|general.principles.DDD.bounded-context]]: The parts circled in blue.
    - language used in each bounded context is specific. For example, "caretaker" of the stables and "caretaker" of the farmhouse are two different things.

## Tactical design tools
Tactical design tools are concerned with implementation details of the components inside a bounded context
tactical design is expected to change over the process of development

# UE Resources
[Event Storming: Brainstorming to arrive at DDD conclusions](https://www.youtube.com/watch?v=b6D_NTgzmhs)
DDD is not about writing code in a certain way. Rather, it's about understanding the underlying logic behind the problems you need to solve. It's the thinking about domains that drives the design of the code. In a way, DDD is the opposite of jumping right in with code and trying to solve something without thinking about it beforehand.
- "The novice jumps in right away with code. The veteran ponders the problem they are solving, and thinks (in code) about the logic of that problem"

DDD is all about unifying all of the code in a way that is highly comprehensible to those uninvolved with the code's creation.
