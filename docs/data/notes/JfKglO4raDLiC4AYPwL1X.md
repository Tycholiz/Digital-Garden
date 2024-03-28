
An entity is an object that...
- can be uniquely identified with an ID
- represent a real world object (generally speaking)
- generally exist as a row in a database table (note the difference between the concept of an apple, vs an actual apple. If your data was modeled with SQL, the latter would make reference to the former)
- typically mutable
- implement some kind of business logic
- consists of [[value objects|general.objects.value-object]]

An entity may also refer to a set of units (e.g. `table` in SQL, `collection` in Mongo, `document` in CouchDB)
- TypeORM defines Entities this way.