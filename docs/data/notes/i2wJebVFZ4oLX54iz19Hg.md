
Stands for **Extract**, **Transform**, **Load**

The ETL Pipeline can be thought as a series of processes whose goal is to take data from some external source, transform it to fit our needs, then loading that transformed data into our own database. 
- With this capability, we are able to enhance reporting, analysis and data synchronization.

### Extract
- data might be extracted from business systems, APIs, data from physical sensors, marketing tools, transaction databases (eg. Stripe)

### Transform
data is temporarily stored in at least one set of staging tables as part of the ETL process

### Load
the load phase doesn't have to be the end of the pipeline. Once the data has been successfully inserted into our database, it can trigger webhooks in other systems to perform more actions.

## Implementations
- [[Apache Flink|flink]]
