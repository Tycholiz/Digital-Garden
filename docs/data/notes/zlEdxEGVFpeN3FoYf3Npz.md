
A cursor is a piece of data (likely just an ID) that points to a location in a paginated list (ie. a single page).
- the cursor is the thing that allows a user to traverse over records in a database

A cursor conceptually has a `next()` method (which gets the next page of the larger dataset) and `forEach()` (which allows some code to be exectuted for each page of the whole dataset).
- cursors may also implement helper methods like `toArray()`, which will allow us extract the data in an array format. 

A cursor can be viewed as a pointer to one (or many) row(s) within a larger dataset.

Cursors also facilitate retrieval of the records, as well as creating and removing records

A cursor is conceptually and behaviourally very similar to an [[general.patterns.behavioural.iterator]]

Cursors are used to process individual rows returned by database system queries

a cursor makes it possible to define a result set and perform complex logic on it, on a row by row basis
