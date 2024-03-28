
## What is it?
The general idea behind indexes is to keep some additional metadata on the side, which acts as a signpost and helps you to locate the data you want. 
- If you want to search the same data in several different ways, you may need several different indexes on different parts of the data.

An index is an additional structure that is derived from the primary data.

Indexes are hidden objects that can exist against a table. You can create an index against a set of columns and the database engine can jump to the specific record based on the index.
- One way of optimizing queries is to create an index on a column or columns you might commonly filter/join against.

a condition in a query is said to be *sargable* if the DBMS engine can take advantage of an index to speed up the execution of the query.
- Sargable operators: =, >, <, >=, <=, BETWEEN, LIKE, IS [NOT] NULL
- sargable -> Search ARGument ABLE

When deciding what indexes to create, you need to consider a number of factors, including your query shapes, query volume, read-to-write ratio, and the size of your database

Think about what happens when you run `CREATE INDEX` to create a new index in a relational database. The database has to scan over a consistent snapshot of a table, pick out all of the field values being indexed, sort them, and write out the index. Then it must process the backlog of writes that have been made since the consistent snapshot was taken (assuming the table was not locked while creating the index, so writes could continue). Once that is done, the database must continue to keep the index up to date whenever a transaction writes to the table.

### Primary Index
Other rows in the database can refer to a row via its primary key. The primary index is used to resolve these references.

### Secondary Index
Imagine you had a bunch of data related to a single user: their `address`, their `favorite_books` etc., all connected by the `user_id` on each of these tables. If we added an index to the `user_id` row, then we could more quickly find all the rows belonging to this single user.

A key-value index maps perfectly to a primary key, since each primary key is unique, but for secondary keys, this isn't the case. To solve this, we can either:
- make each value in the index a list of matching row identifiers (like a postings list in a full-text index) 
- make each key unique by appending a row identifier to it

Secondary indexes are the bread and butter of relational databases, and they are common in document databases too.

A secondary index usually doesn’t identify a record uniquely, but rather is a way of searching for occurrences of a particular value: 
- ex. find all actions by user 123, find all articles containing the word hogwash, find all cars whose color is red etc.

Secondary indexes add considerable complexity when it comes to [[partitioning|db.distributed.partitioning]] our database.
- The problem with secondary indexes is that they don’t map neatly to partitions. With a primary index, we just need to determine which partition the data will belong on based on the primary key.

### Analogy: Umbrella
The metal tips on the edge of an umbrella represent the items that are available for retrieval in a database. If you were not using an index, you'd have to look at each tip to see if it's the item you want or not. With an index, you look from the top of the umbrella, and each arm leading to the tip would light up, indicating which items should be retrieved.
- therefore, indexes basically tell us how to get a certain piece of data.

### Analogy: Textbook
Imagine studying for a test and you stumbled upon a topic you don't understand or forgot. You might consider looking at the INDEX at the back of the textbook for the topic. The index will tell you which page to go find the information. You can then jump straight to the correct page. Now imagine not having an index at the back of the textbook. You'd have to skim through the ENTIRE book until you found what you were looking for.

## Why use it?
Indexing makes querying by a column faster, at the slight expense of create/update speed and database size.
- For example, you will often want to query all comments belonging to a post (that is, query comments by its `post_id` column), and so you should mark the `post_id` column as indexed.
- However, if you rarely query all comments by its author, indexing `author_id` is probably not worth it.
- In general, most `_id` fields are indexed. Sometimes, boolean fields are worth indexing if you often use it for queries. However, you should almost never index date (`_at`) columns or string columns.

Consider the following SQL statement: 
```sql
SELECT first_name FROM people WHERE last_name = 'Smith';
```
To process this statement without an index the database software must look at the `last_name` column on every row in the table (ie. it must perform a full table scan). 
- With an index, the database simply follows the index data structure (typically a [[B-tree|general.lang.data-structs.tree.B]]) until the Smith entry has been found; this is much less computationally expensive than a full table scan.

We should index the things that we are likely to query on. If we frequently want to retrieve books from the db filtered by a certain year, we'd want to add an index onto the "year" column, making that data easier to retrieve.
- otherwise the DBMS would have to search through every single book to see if it satisfies the filter requirements for the year we are seeking.

Querying by an index changes the performance of your query from $O(n)$ to $O(log(n))$
- With 80 million documents, this would mean around 26 operations, rather than 80 million operations

In the case of data sets that are many terabytes in size, but have very small payloads (e.g., 1 KB), indexes are a necessity for optimizing data access. Finding a small payload in such a large dataset can be a real challenge, since we can’t possibly iterate over that much data in any reasonable time.
- Furthermore, it is very likely that such a large data set is spread over several physical devices—this means we need some way to find the correct physical location of the desired data. Indexes are the best way to do this.

An index allows the system to have new options to find the data that your queries need
- In the absence of an index, the only option available to your database is a sequential scan of your tables. The index access methods are meant to be faster than a sequential scan, by fetching the data directly where it is.

Indexes add overhead, especially on writes.
- The index also needs to be updated every time data is written.
- consider that it's going to be difficult to out-perform simply appending to a file (as opposed to random writes)
- anal: Using a knowledge-management system like Dendron comes at a cost of write speed, since we need to consider where we want to store the note. If we wanted to optimize "write speed" and nothing else, we would just put all of our notes in a single massive text file. However, since we care about retrieval, we sacrifice write speed for the benefit of retrieving it faster. This is the concept of an Index.

Depending on whether our data store is [[OLAP|db.olap-oltp]] or OLTP, our indexing strategies will be very different.

## How does it work?
The key in an index is the thing that queries search for, but the value can be one of two things: 
1. it could be the actual row (document, vertex) in question
2. it could be a reference to the row stored elsewhere
  - this "elsewhere" is known as a *heap file*

## Types
### Hash Index
This is a common index employed with key-value data.

The index itself is a key-value index

This indexing strategy works best when there are lots of writes, but not too many distinct keys (so that keeping it all in-memory is feasible)

Imagine our database was a simple append-only file of key-value pairs. To implement indexing, we could have a [[hash map|general.lang.data-structs.hash-table]].

Every time we append a new entry to the database file, 

Imagine our database file was:
```json
123456:{
  "name":"London",
  "attractions": ["Big Ben","London Eye"]
} 
42:{ 
  "name":"San Francisco",
  "attractions": ["Golden Gate Bridge"]
}
```

All we need to do is minify it (remove all spaces and newlines), and then make our index hash map as a series of key-value pairs, where each key maps to a byte offset of characters from the start.
- ex. key `123456` maps to `0`, since it is at the start. Key `42` maps to `64`, since it is 64 bytes (characters) from the start:
```json
{
  123456: 0,
  42: 64
}
```

### B-Tree
[[general.lang.data-structs.tree.B]]

B-tree is a key-value index.

### Clustered Index
A clustered index determines the physical order of data in a table. 
- Therefore, a table has only one clustered index (primary key/composite key).

Clustered indexes are indexes whose order of the rows in the data pages corresponds to the order of the rows in the index

A clustered index defines the order in which data is physically stored in a table

Having a clustered index involves storing all row data within the index

### Non-clustered Index
Having a non-clustered index involves storing only references to the data (the heap file) within the index

A non clustered index is analogous to an index in a Book. The data is stored in one place. The index is stored in another place and the index has pointers to the storage location. this help in the fast search of data. For this reason, a table has more than 1 Nonclustered index.

### Multi-column Index
The previous indexes only map a single key to a value, but what if we need to query multiple columns of a table simultaneously?

#### Concatenated Index
Most common type of Multi-column index is called a *concatenated index*
- works by simply combining several fields into one key by appending one column to another
- anal: a phone book uses the format `last name, first name` as an index. This provides benefits to quick searching by last name. However, the drawback is that order matters. Therefore, it is trivial to find out how many Smiths (ie. last name) there are in total, but it is very difficult to find out how many Johns (ie. first name) there are. 

### Multi-dimensional Index
Allow us to more easily query several columns at once. In other words, it allows us to narrow down our query based on multiple columns at once, rather than having to search by a single column, then filter out the ones that don't find our query.

#### Example: location queries
imagine we had an application using Google maps, and depending on the user's current zoom radius, we need to query the database for locations within that area.
- this is an example of a two-dimensional range query
```sql
SELECT * FROM restaurants 
  WHERE latitude > 51.4946 
    AND latitude < 51.5079 
    AND longitude > -0.1162 
    AND longitude < -0.1004;
```

A standard B-tree or LSM-tree index cannot perform this query efficiently. By its nature, it would make us choose between:
- all the restaurants in a range of latitudes (but at any longitude)
- all the restaurants in a range of longitudes (but anywhere between the North and South poles)

#### Other examples
- on an e-commerce website, we could allow the user to query for products in a certain range of color (e.g. only red, green or blue)
- in a weather database, we could allow the user to query on both `date` AND `temperature`, giving us the ability to search for all observations of 40+ degree weather within 2020.
  - to do this with single-dimensional indexes, we'd have to scan over *all* the records from 2020 and then filter them by temperature (or vice versa)

### Full-text search and Fuzzy Indexes
These type of indexes are good if we don't know exactly what we are searching for (ie. we are searching for similar keys, such as misspelled words)

Consider the allowances that a search engine gives for a full-text search query:
- synonyms are included
- regional spelling differences are ignored (e.g. organization vs organisation)
- vicinity of the searched word to other searched words in the same document