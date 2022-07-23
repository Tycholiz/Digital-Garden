
ElasticSearch is an open-source, RESTful, distributed search and analytics engine built on Apache Lucene
- You can send data in the form of JSON documents to Elasticsearch using the API. Elasticsearch automatically stores the original document and adds a searchable reference to the document in the cluster’s index. You can then search and retrieve the document using the Elasticsearch API
- ES is NoSQL and is more powerful, flexible, and faster than SQL's LIKE

ES is typically used when you have :
- high data volumes, and are likely to need multiple nodes to process the data
- unstructured or semi-structured data (log files, text, ...). You ingest the raw data in its original form.
- the data is never updated. It’s ingested once, queried, and then purged according to some bulk retention policy (e.g. older than 30 days)
- you need to access aggregate data more than individual records

When to use ElasticSearch?
- If your use case requires a full-text search, Elasticsearch will be the best fit
- If your use case involves chatbots where these bots resolve most of the queries, such as when a person types something there are high chances of spelling mistakes. You can make use of the in-built fuzzy matching practices of the ElasticSearch
- Also, ElasticSearch is useful in storing logs data and analyzing it

When you're searching for text. ES ranks search results based on how close the phrase or words are. SQL doesn't do this nearly as well.
- ES starts to shine when you start to do a lot of filtering

Elastic search scales horizontally with your requirements.

Secondary [[indexes|db.strategies.index]] are the raison d’être of search servers such as Elasticsearch.

# Tools
[Kibana: a data visualization platform for Elasticsearch](https://www.elastic.co/kibana)
