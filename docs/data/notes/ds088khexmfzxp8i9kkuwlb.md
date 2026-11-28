
### Query
```sh
curl -X GET "https://localhost:9200/index_name/_search?q=<query_string>" \
    -H "Authorization: ApiKey "${API_KEY}"" \
    -H "Content-Type: application/json"
```