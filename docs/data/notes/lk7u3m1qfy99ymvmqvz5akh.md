
A Repository is basically a mechanism where you let Spring write the database queries for you based on (for example) the method names.

If you have a `PersonRepository` defined properly, with a method defined like this:

```java
List<Person> findByFirstName();
```

Under the hood Spring will generate code for you that would turn this into a query like:

```sql
SELECT * FROM person WHERE firstName = :firstName
```

And then also do the mapping of the query results to `Person` objects for you.