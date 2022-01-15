
Indexing makes querying by a column faster, at the slight expense of create/update speed and database size.
- For example, you will often want to query all comments belonging to a post (that is, query comments by its `post_id` column), and so you should mark the `post_id` column as indexed.
- However, if you rarely query all comments by its author, indexing `author_id` is probably not worth it.
- In general, most `_id` fields are indexed. Sometimes, boolean fields are worth indexing if you often use it for queries. However, you should almost never index date (`_at`) columns or string columns.

In the case of data sets that are many terabytes in size, but have very small payloads (e.g., 1 KB), indexes are a necessity for optimizing data access. Finding a small payload in such a large dataset can be a real challenge, since we can’t possibly iterate over that much data in any reasonable time.
- Furthermore, it is very likely that such a large data set is spread over several physical devices—this means we need some way to find the correct physical location of the desired data. Indexes are the best way to do this.

Indexes are hidden objects that can exist against a table. You can create an index against a set of columns and the SQL engine can jump to the specific record based on the index.
- One way of optimizing queries is to create an index on a column or columns you might commonly filter/join against.

- spec: so we should index the things that we are likely to query on. If we frequently want to retrieve books from the db filtered by a certain year, we'd want to add an index onto the "year" column, making that data easier to retrieve.
	- spec: otherwise we'd have to essentially search through every single book to see if it satisfies the filter requirements for the year we are seeking.

An index allows the system to have new options to find the data that your queries need
- In the absence of an index, the only option available to your database is a sequential scan of your tables. The index access methods are meant to be faster than a sequential scan, by fetching the data directly where it is.

### Umbrella Analogy
The metal tips on the edge of an umbrella represent the items that are available for retrieval in a database. If you were not using an index, you'd have to look at each tip to see if it's the item you want or not. With an index, you look from the top of the umbrella, and each arm leading to the tip would light up, indicating which items should be retrieved.
- therefore, indexes basically tell us how to get a certain piece of data.

### Textbook Analogy
Imagine studying for a test and you stumbled upon a topic you don't understand or forgot. You might consider looking at the INDEX at the back of the textbook for the topic. The index will tell you which page to go find the information. You can then jump straight to the correct page. Now imagine not having an index at the back of the textbook. You'd have to skim through the ENTIRE book until you found what you were looking for.
