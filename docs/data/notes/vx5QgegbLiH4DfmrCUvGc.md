
### Copy from CSV into Postgres table
1. In psql, create a new table that matches the format of the csv headers
2. In psql, run the COPY command:
```sql
COPY cars
FROM '/Users/kyletycholiz/Downloads/cars.csv' 
DELIMITER ',' 
CSV HEADER;
```
note: if we don't want to have the table columns match the csv headers, we can specify like so:
```sql
COPY cars(id, name)
...
```
