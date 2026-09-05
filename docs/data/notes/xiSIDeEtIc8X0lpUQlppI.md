
### What is it?
ElasticSearch is an open-source, RESTful, distributed search and analytics engine built on Apache Lucene
- You can send data in the form of JSON documents to Elasticsearch using the API. 
  - Elasticsearch automatically stores the original document and adds a searchable reference to the document in the cluster’s index. You can then search and retrieve the document using the Elasticsearch API
- due to its distributed nature, documents are available on all nodes of the cluster.
  - Each document in an index belongs to one primary shard, but is replicated amongst the other shards.
    - ES selects the shards that the query should go to in a round-robin fashion
- ES is NoSQL and is more powerful, flexible, and faster than SQL's LIKE
- ES Documents are heavily [[denormalized|db.design.normalization]], resulting in documents that do not reference one another.

#### Example Reddit post as ElasticSearch document:
```json
{
  "id": "abcdefg",
  "title": "Amazing subreddit for nature lovers!",
  "content": "Hey everyone!\n\nI just stumbled upon this incredible subreddit called NatureIsBeautiful and I can't stop scrolling through the posts.",
  "author": "nature_enthusiast23",
  "created_at": "2023-06-05T14:30:00Z",
  "subreddit": "NatureIsBeautiful",
  "upvotes": 1500,
  "comments": 87,
  "tags": ["nature", "photography", "community"],
  "url": "https://www.reddit.com/r/NatureIsBeautiful/comments/abcdefg/amazing_subreddit_for_nature_lovers/"
}
```

### Why use it?
ES is typically used when you have:
- high data volumes, and are likely to need multiple nodes to process the data
- unstructured or semi-structured data (log files, text, ...). You ingest the raw data in its original form.
- the data is treated as a blob, and thus never updated. It’s ingested once, queried, and then purged according to some bulk retention policy (e.g. older than 30 days)
- you need to access aggregate data more than individual records
- you need to index in real time, allowing you ingest high-throughput data streams and query that data quickly, making it well-suited for applications that require constant updates and querying of rapidly changing data

When to use ElasticSearch?
- If your use case requires a full-text search, including features like fuzzy matching, stemming (e.g. having the word "run" also match "runs", "running" etc), and relevance scoring.
- If your use case involves chatbots where these bots resolve most of the queries, such as when a person types something there are high chances of spelling mistakes. You can make use of the in-built fuzzy matching practices of the ElasticSearch
- Also, ElasticSearch is useful in storing logs data and analyzing it

Other use cases:
- Add a search box to an app or website
- Store and analyze logs, metrics, and security event data
- Use machine learning to automatically model the behavior of your data in real time
- Automate business workflows using Elasticsearch as a storage engine
- Manage, integrate, and analyze spatial information using Elasticsearch as a geographic information system (GIS)
- Store and process genetic data using Elasticsearch as a bioinformatics research tool

Elastic search scales horizontally with your requirements.

Forms part of the ELK stack (along with Logstash and Kibana), giving us log analysis, monitoring, and visualization in the context of application and server logs.

#### As part of the ElasticStack (ELK)
ELK consists of ElasticSearch, Kibana, Beats and Logstash
- *Logstash* and *Beats* facilitate collecting, aggregating, and enriching your data and storing it in Elasticsearch
- *Kibana* enables you to interactively explore, visualize, and share insights into your data and manage and monitor the stack.
- *Elasticsearch* is where the indexing, search, and analysis magic happens.

### How does it work?
When you're searching for text. ES ranks search results based on how close the phrase or words are. SQL doesn't do this nearly as well.
- ES starts to shine when you start to do a lot of filtering

Elasticsearch chooses the best underlying data structure to use for a particular field type. 
- Text is tokenized and stored in an inverted index, which supports very fast full-text searches.
  - an inverted index lists every unique word that appears in any document and identifies all of the documents each word occurs in.
  - ex. if we search for the string `London`, it is the inverted index that allows us to quickly know that the string occurs in 6 different documents in the index.
- Numeric and geolocational data is stored in BKD trees
  - this allows for fast-range searches and nearest-neighbor queries in large data sets

Secondary [[indexes|db.strategies.index]] are the raison d’être of search servers such as Elasticsearch.

Mapping is the process by which ES determines how a document is stored and indexed.

#### How data is retrieved
Based on the query terms passed, each document retrieved will be assigned a score. The documents are then returned to the client sorted by that score.
- this is [the BM25 algorithm](https://www.elastic.co/blog/practical-bm25-part-2-the-bm25-algorithm-and-its-variables)
- note: if I pass "prescription refill", then ES recognizes that there are 2 terms: `prescription` and `refill`

Some factors that determine the document's score:
- *rarity* - queries that contain rarer terms (amongst *all* documents) have a higher multiplier, meaning they contribute more to the final score
  - ex. the word "the" is likely to be very common amongst all matching documents, while the word "elephant" likely to be rare. As a result, ES recognizes that the word "elephant" is more important, and makes its contribution to the final document's score higher.
  - this is known as *Inverse Document Frequency (IDF)*
- *density* - documents that are longer than average will have the score penalized. 
  - That is, the more terms in the document (ones that don't match the query), the lower the score for the document.
  - expl: this makes intuitive sense: if a document is 300 pages long and mentions the word elephant once, the document is more likely to have said something like "elephant in the room", rather than it actually being a document about elephants. On the other hand, if the document is a tweet of 140 characters, then the word Elephant is much more likely to have actually been about Elephants.
  - this is known as *Term Frequency (TF)*

In the absense of replicas, a given query and set of documents will result in a more-or-less deterministic result
- this non-determinism resulting from replicas happens because ES determines which shard the query should go to in a round-robin fashion, so the same query run twice in a row will likely go to different copies of the same shard.

### How to use it?
#### Searching data
The Elasticsearch REST APIs support structured queries, full text queries, and complex queries that combine the two.
- *Structured queries* are similar to the types of queries you can construct in SQL. 
  - ex. you could search the gender and age fields in your employee index and sort the matches by the hire_date field. 
  - [Query SDL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html), [ElasticSearch SQL](https://www.elastic.co/guide/en/elasticsearch/reference/current/sql-overview.html)
- *Full-text queries* find all documents that match the query string and return them sorted by relevance—how good a match they are for your search terms.

#### Performing aggregations
Aggregations enable you to build complex summaries of your data and gain insight into key metrics, patterns, and trends.

Instead of just finding the proverbial “needle in a haystack”, aggregations enable you to answer questions like:
- How many needles are in the haystack?
- What is the average length of the needles?
- What is the median length of the needles, broken down by manufacturer?
- How many needles were added to the haystack in each of the last six months?
- What are your most popular needle manufacturers?
- Are there any unusual or anomalous clumps of needles?

Because aggregations leverage the same data-structures used for search, they are also very fast.

* * *

## ElasticSearch Primitives
Comparison to RDBMS
- RDBMS => Databases => Tables => Columns/Rows
- Elasticsearch => Clusters => Indices => Shards => Documents

### Index
An Elasticsearch index is a logical namespace that holds a collection of documents
- That is, an ES Index has nothing to do with [[database indexes|db.strategies.index]], and are more comparable to tables in SQL
- "indexing a document" means "inserting a document into the index"

### Index Mapping
essentially a schema for how data will be structured in the index

Each field in a mapping has an analyzer associated with it

### Analyzer
Each analyzer contains:
- a tokenizer
- a normalizer
- filters

ES has built-in analyzers, but we can define custom ones, where we define our own tokenizer

### Tokenizer
- converts text into tokens
  - ex. converts "a quick brown fox jumps over the lazy dog" into terms `["a", "quick", "brown"]` etc.

Tokenizer types:
- word-oriented
- partial-word
- structured text

#### N-gram tokenizer
can break a word up into a sliding window of continuous letters
- ex. "quick" -> ["qu", "ui", "ic", "ck"]

#### Edge N-gram tokenizer
- ex. "quick" -> ["q", "qu", "qui", "quic", "quick"]

### Filter
might do things like removing articles from the terms (e.g. `a`, `the`), or do things like include derivate words in the search (e.g. cleaner -> `["cleaning", "cleaned", "cleans"]`), or a synonym filter, which adds matches for synonyms that may appear.

### Normalizer
A special type of analyzer
- emits a single token for a given input, instead of an array of tokens

* * *

## Queries
Compound vs Leaf queries
- leaf query matches against a specific field
- compound query combine leaf queries in various ways 

Type of compound queries
- bool (ex. `should`, `must_and` etc.)
- boosting
- constant_score
- dis_max - only the highest score of any leaf query within a compound query will be considered
- Function_score - allow us to use more complex functions to determine the score

Leaf queries can have their scores boosted with multipliers

### Full-text Query
A type of leaf query

Matches against text in a specific field

`match` is the most common type of full-text query


# Tools
- [Kibana: a data visualization platform for Elasticsearch](https://www.elastic.co/kibana)
