
Databases fall into 2 main categories: those fit for handling business logic, and those fit for handling business analytics.

OLAP are used for business analysis, and feed into reports that help the management of a company make better decisions (ie. business intelligence)

OLTP are used to handle business logic in the application.

| Property             | Transaction processing systems (OLTP)             | Analytic systems (OLAP)                   |
|----------------------|---------------------------------------------------|-------------------------------------------|
| Main read pattern    | Small number of records per query, fetched by key | Aggregate over large number of records    |
| Main write pattern   | Random-access, low-latency writes from user input | Bulk import (ETL) or event stream         |
| Primarily used by    | End user/customer, via web application            | Internal analyst, for decision support    |
| What data represents | Latest state of data (current point in time)      | History of events that happened over time |
| Dataset size         | Gigabytes to terabytes                            | Terabytes to petabytes                    |

In the past, both OLTP and OLAP operations were done on the same SQL databases, but soon after it was realized that analytics data should be stored in its own database. This is what are known as [[data warehouses|data.warehouse]] today.
