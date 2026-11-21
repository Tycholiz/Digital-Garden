
### Primary key
DynamoDB stores and retrieves each item based on the primary key value

Can be either:
- Simple primary key, composed solely of the *partition key*
- Composite primary key, composed of the *partition key* and the *sort key* (a.k.a. hash attribute and range attribute)

![](/assets/images/2023-01-25-13-43-58.png)

### Global Secondary Index (GSI)
[Overview](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html)

The idea is that we create GSIs and then we issue Query requests against them. 
- without GSIs, we'd only be able to query against the primary key, which is highly limiting our potential to query against business logic.
- GSIs are what makes JOINs unnecessary in DynamoDB.
    expl: a RDBMS does a lot of the work in figuring out how to perform the query you give it. A NoSQL db like Dynamo would instead have us define how we'd like to access our data beforehand, then add GSIs that define specifically how to access that data. From there, the user just needs to query the GSI and it will get all of the data it needs
- ex. if we had a table that stored books and authors, we could have a GSI whose business logic would encapsulate the idea of "all books belonging to one author".

A GSI contains a selection of attributes from the base table, but they are organized by a primary key that is different from that of the table.

We can have multiple GSIs, with each corresponding essentially to a particular unit of business logic.
- ex. `BooksByAuthor-Index` and `AuthorsByBook-Index` would be two different GSIs, effectively allowing us to query all books by a single author, and all authors of a single book.

[[see also|db.distributed.partitioning#term-based,1]]

#### Partition Key
- a.k.a *hash key*, *hash attribute*
    - The term *hash attribute* derives from the use of an internal hash function in DynamoDB that evenly distributes data items across partitions, based on their partition key values.

Most often the partition key is the `id` attribute of an item

Dynamo uses the partition key as input to the hashing function which determines which partition the data will live on.

- All items with the same partition key value are stored together, in sorted order by sort key value.
- ex. in booksByAuthors example, `hash_key = book`


#### Sort key
- a.k.a *range key*, *range attribute*
    - The term *range attribute* derives from the way DynamoDB stores items with the same partition key physically close together, in sorted order by the sort key value.

Determines the attribute that the results (ie. projection) will be grouped by.
- ex. in booksByAuthors example, `range_key = author`

Providing a sort key (along with a partition key) creates a composite primary key, which gives you additional flexibility when querying data
- ex. if you provide only the value for `Artist`, DynamoDB retrieves all of the songs by that artist. To retrieve only a subset of songs by a particular artist, you can provide a value for `Artist` along with a range of values for `SongTitle`.
    `range_key = SongTitle`

Well-designed sort keys have 2 major benefits:
1. related information is gathered together in one place where it can be queried efficiently. 
    - This lets us retrieve commonly needed groups of related items using range queries with operators such as begins_with, between, >, <, and so on.
2. Composite sort keys let you define hierarchical (one-to-many) relationships in your data that you can query at any level of the hierarchy.
    - ex. in a table listing geographical locations, you might structure the sort key: `[country]#[region]#[state]#[county]#[city]#[neighborhood]`. This would let you make efficient range queries for a list of locations at any one of these levels of aggregation, from country, to a neighborhood, and everything in between.

#### `projection_type`
Specifies which attributes we want returned from the query. 

Can be `INCLUDE` or `ALL`. 
- If `INCLUDE`, then we must specify a list of attributes we want (`non_key_attributes`)
