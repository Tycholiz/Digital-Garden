
a repository allows you to populate an application with data that comes from the database in the form of the domain entities. Once the entities are loaded, they can be changed and then persisted back to the database through transactions.

The repository pattern has 2 purposes:
1. abstract the data layer
2. centralize the handling of domain objects. 

A repository is a class / component that encapsulates the logic required to access [[data sources|general.patterns.data-source]]

If executed properly, this pattern will relieve any consuming code of knowing what data layer is being used under the covers. 
- Client objects declaratively build queries and send them to the repositories for answers.
- a consumer that uses a repository should have no knowledge if the underlying layer is an SQL database, a NoSQL database, or even just an online API that does some further interaction.

Often the data layer logic / knowlege that is hidden is performing some set of CRUD actions. In these cases, a repository might be implemented as a class with methods that provide logic to access the database.

We should create one repository per [[aggregate|general.principles.DDD.aggregate]]
- [source](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design#define-one-repository-per-aggregate)
- the purpose is to achieve [[transactional|db.strategies.transaction]] consistency amongst all objects of the aggregate.
  - if instead we decided to create an aggregate for each table, we would be making our data-operations less transactional.

Consider the overlap between an [[ORM|sql.ORM]] and a repository. An ORM abstracts away the "how" of how we interact with a set of data. An ORM operates at a slightly higher level of abstraction, becuase it abstracts away the logic of how data is actually accessed 
- with an ORM used over SQL, we don't write a `SELECT *`, we write `findAll()`. With a repository, we still have to write the `SELECT *`, but we get the benefits of abstracting that fact away from those who consume the repository.

Question: what is the difference between a data source and a repository?