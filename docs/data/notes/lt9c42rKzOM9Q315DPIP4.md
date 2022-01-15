
Allows us to store and manipulate a period of time.
```sql
interval '2 months ago';
interval '3 hours 20 minutes';
interval '2 month - 1 day';
```

Internally, PostgreSQL stores interval values as months, days, and seconds. The months and days values are integers while the seconds can field can have fractions.

The interval values are very useful when doing date or time arithmetic. For example, if you want to know the time of 3 hours 2 minutes ago at the current time of last year, you can use the following statement:
```sql
SELECT
	now(),
	now() - INTERVAL '1 year 3 hours 20 minutes'
             AS "3 hours 20 minutes ago of last year";
```

### Proper types for selected columns
```sql
select album.title as album,
	sum(milliseconds) * interval '1 ms' as duration
	...
```
