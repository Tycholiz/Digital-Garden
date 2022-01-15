
An ORM is for translating SQL records into Python objects (or whatever language used on the server). If you think it can handle anything more, like preventing your need to write SQL, managing indexes etc… you are wrong.

If our server has an ORM built-in (like the one Django has), it is not necessarily a bad idea to use an ORM. Consider that:
```py
MyModel.objects.get(id=1)
```

is equivalent to:
```sql
select mymodel.id, mymodel.other_field, ...
  from mymodel
 where id=1;
 ```

The SQL language abstracts away the procedural steps for querying a database, allowing one to merely define what one wants. But certain SQL queries are thousands of times slower than other logically equivalent queries. On an even higher level of abstraction, ORM systems, which isolate object-oriented code from the implementation of object persistence using a relational database, still force the programmer to think in terms of databases, tables, and native SQL queries as soon as performance of ORM-generated queries becomes a concern.
