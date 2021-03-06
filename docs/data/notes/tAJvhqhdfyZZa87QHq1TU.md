
Objection is a relational query builder. 
- get all the benefits of an SQL query builder but also a powerful set of tools for working with relations.

# Parts
- define models declaratively, and map the relationship between models (1:many etc)
- QueryBuilder - Every method that allows you to fetch or modify items in the database returns an instance of the QueryBuilder
	- therefore most important component in Objection

All instance methods start with the character $ to prevent them from colliding with the database column names.

## Model
consider that in an app, there are different layouts of data:
1. it could be the layout that exists in the database itself
2. it could be the layout that exists when the user gets that data back from the database (ie, on the client)
therefore, any time we read or write to a database, we are converting data

there are 4 methods on a model that are used to transform data. They exist by default, but can be overridden. 
these "converter methods" will be called each time data is tranformed: 
1. when we are writing data, we are converting it to the database layout, therefore `$formatDatabaseJson`
2. when we are reading data, we are converting it to our internal data layout, therefore `$parseDatabaseJson`
3. when we give data for a query, (for example `query().insert(req.body)`) or create a model explicitly using `model.fromjson(obj)` the `$parsejson` method is invoked
	- When you call `model.toJSON()` or `model.$toJson()` the `$formatJson` is called.
- Note: Most libraries like express and koa automatically call the toJSON method when you pass the model instance to methods like response.json(model). You rarely need to call toJSON() or $toJson() explicitly.
- All properties that start with $ are also removed from database and external layouts.
