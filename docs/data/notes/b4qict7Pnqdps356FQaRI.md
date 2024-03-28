
key:value pairs that enable user-defined annotation of spans in order to query, filter, and comprehend trace data.

Examples may include tag keys like db.instance to identify a database host, http.status_code to represent the HTTP response code, or error which can be set to True if the operation represented by the Span fails.

#### Example
- db.instance:"customers"
- db.statement:"SELECT * FROM mytable WHERE foo='bar'"
- peer.address:"mysql://127.0.0.1:3306/customers"
