
Key–value databases work in a very different fashion from the better known relational databases (RDB)
- RDBs predefine the data structure in the database as a series of tables containing fields with well defined data types. Exposing the data types to the database program allows it to apply a number of optimizations.
- In contrast, key–value systems treat the data as a single opaque collection, with no unified structure to the value of each record.
  - this more closely follow the principles of [[OOP|paradigm.oop]]

key–value databases often use far less memory than RDBs to store the same data
- This is because optional values are not represented by placeholders or input parameters, as they are in most RDBs