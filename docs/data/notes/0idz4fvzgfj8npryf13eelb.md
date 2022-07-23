
### Primary key
DynamoDB stores and retrieves each item based on the primary key value

Can be either:
- Simple primary key, composed solely of the *partition key* (most often the `id` attribute of an item)
- Composite primary key, composed of the *partition key* and the *sort key* (ie. hash attribute and range attribute)
    - The term *hash attribute* derives from the use of an internal hash function in DynamoDB that evenly distributes data items across partitions, based on their partition key values.
    - The term *range attribute* derives from the way DynamoDB stores items with the same partition key physically close together, in sorted order by the sort key value.

### Global Secondary Index (GSI)
[Overview](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html)

The idea is that we create GSIs and then we issue Query requests against them. 
- without GSIs, we'd only be able to query against the primary key, which is highly limiting our potential to query against business logic.
- GSIs are what makes JOINs unnecessary in DynamoDB.
- ex. if we had a table that stored books and authors, we could have a GSI whose business logic would encapsulate the idea of "all books belonging to one author".

A GSI contains a selection of attributes from the base table, but they are organized by a primary key that is different from that of the table.

We can have multiple GSIs, with each corresponding essentially to a particular unit of business logic.
- ex. `BooksByAuthor-Index` and `AuthorsByBook-Index` would be two different GSIs, effectively allowing us to query all books by a single author, and all authors of a single book.

[[see also|db.distributed.partitioning#term-based,1]]

#### Partition Key (a.k.a `hash_key`)
it decides where in the table the data will live.
- ex. in booksByAuthors example, `hash_key = book`

#### Sort key (a.k.a `range_key`)
Determines the attribute that the results (ie. projection) will be grouped by.
- ex. in booksByAuthors example, `range_key = author`

Well-designed sort keys have 2 major benefits:
1. related information is gathered together in one place where it can be queried efficiently. 
    - This lets us retrieve commonly needed groups of related items using range queries with operators such as begins_with, between, >, <, and so on.
2. Composite sort keys let you define hierarchical (one-to-many) relationships in your data that you can query at any level of the hierarchy.
    - ex. in a table listing geographical locations, you might structure the sort key: `[country]#[region]#[state]#[county]#[city]#[neighborhood]`. This would let you make efficient range queries for a list of locations at any one of these levels of aggregation, from country, to a neighborhood, and everything in between.

#### `projection_type`
Specifies which attributes we want returned from the query. 

Can be `INCLUDE` or `ALL`. 
- If `INCLUDE`, then we must specify a list of attributes we want (`non_key_attributes`)
