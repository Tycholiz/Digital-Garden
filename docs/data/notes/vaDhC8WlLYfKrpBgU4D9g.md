
Command Query Responsibility Segregation (CQRS) is an architectural pattern that separates reading and writing into two different models. This means that every method should either be a Command that performs an action or a Query that returns data. A Command cannot return data and a Query cannot change the data
- ex. Redux attempts to follow this pattern. Consider that to write to state we must dispatch an action, and to read state we must use selectors. The implementations of reading and writing to state are decoupled.

# UE Resources
https://martinfowler.com/bliki/CQRS.html
