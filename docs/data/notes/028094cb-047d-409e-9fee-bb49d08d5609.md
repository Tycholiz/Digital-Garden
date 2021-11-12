
# What is relational?
A relation is a set of tuples (fixed length array)
- in other words, a single row in a single database is a relation between all the values that represent that row
- in SQL, a relation is represented by a table, with each row being a tuple
- a tuple is described as a function, since it maps names to values.
	- ex. for row 1, name -> "Kyle"
- each element (cell) of the tuple is termed an `attribute value`
	- an attribute is the combination of the column name and its type
		- ex. age int
	- an attribute value is the cross-section of an attribute and a tuple (ie. it is a single piece of data)
- each SQL clause will transform one or several relations in order to produce new relations.
- Data is not relational. Rather, data has relationships.
- *relational* is the concept that we have a set of elements that all share the same properties (aka attribute domains)
	- ex. if we have a table `foo` with 2 columns: id: int and name: text, then every single record in this database will look exactly like that.
