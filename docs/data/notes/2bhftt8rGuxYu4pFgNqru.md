
SQL is a declarative language— we declare the result we want to obtain in terms of a data processing pipeline that is executed against a known database model and a dataset.

Every single sql query contains some level of business logic

The SQL language is statically typed: every query defines a new relation that must be fully understood by the system before executing it. This is the reason why sometimes cast expressions are needed in your Queries

In SQL, you need to explain your problem, unlike most programming languages where you need to focus on a solution you think is going to solve your problem. When struggling to write an sql query, First write down a single sentence in English that perfectly describes what you’re trying to achieve.

## Parts of SQL
All SQL can be broken down into either DDL or DML
- both DDL and DML have their own CRUD operations

### DDL (data definition language)
- used to define data structures (working with the structure of the data)
    - associated with creating tables, defining relationships (primary, foreign and composite keys, for example) and data types, constraints, etc, and also for modifying them.
- ex. `CREATE`, `ALTER`, `DROP`

### DML (data manipulation language)
- used to manipulate data (working with the data itself)
- ex. `INSERT`, `UPDATE`, `DELETE`, `SELECT`

# E Resource
[quality tutorial (current progress)](https://pgexercises.com/questions/joins/self2.html)
[quality tips](https://blog.jooq.org/2016/03/17/10-easy-steps-to-a-complete-understanding-of-sql/)
