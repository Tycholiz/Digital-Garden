
Elasticsearch is an open-source, RESTful, distributed search and analytics engine built on Apache Lucene
- You can send data in the form of JSON documents to Elasticsearch using the API. Elasticsearch automatically stores the original document and adds a searchable reference to the document in the cluster’s index. You can then search and retrieve the document using the Elasticsearch API
- ES is NoSQL and is more powerful, flexible, and faster than SQL's LIKE

ES is typically used when you have :
- high data volumes, and are likely to need multiple nodes to process the data
- unstructured or semi-structured data (log files, text, ...). You ingest the raw data in its original form.
- the data is never updated. It’s ingested once, queried, and then purged according to some bulk retention policy (e.g. older than 30 days)
- you need to access aggregate data more than individual records

When you're searching for text. ES ranks search results based on how close the phrase or words are. SQL doesn't do this nearly as well.
- ES starts to shine when you start to do a lot of filtering

Elastic search scales horizontally with your requirements.

# Tools
[Kibana: a data visualization platform for Elasticsearch](https://www.elastic.co/kibana)
