
# DBMS
*"SQL tells the database what information you want, and the DBMS (Postgres, MySQL) determines the best way to provide it."*
- its role in your software architecture is to handle concurrent access to live data that is manipulated by several applications, or several parts of an application.

When a select query is made, The database translates the query into a "query plan" which may vary between executions, database versions and database software. This functionality is called the "query optimizer" as it is responsible for finding the best possible execution plan for the query, within applicable constraints.

Goal of a DBMS is to handle concurrent access to live data that is manipulated by several applications, or several parts of an application
- at the core of this is the concept of a transaction (RDBMS)

### Object–relational Impedance Mismatch
The object–relational impedance mismatch is a set of conceptual and technical difficulties that are often encountered when a relational database management system (RDBMS) is being served by an application program (or multiple application programs) written in an object-oriented programming language or style, particularly because objects or class definitions must be mapped to database tables defined by a relational schema.


# UE Resources
- [Evolutionary database design](https://martinfowler.com/articles/evodb.html)
