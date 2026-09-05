
For timestamp with time zone, the internally stored value is always in UTC
- An input value that has an explicit time zone specified is converted to UTC using the appropriate offset for that time zone.
	- If no time zone is stated in the input string, then the system's TimeZone parameter is used.

`+00:00` indicates an `hour:minute` timezone offset

### Timezone
Timezone can be changed like:
- `set timezone to 'Europe/Paris';`

### Timestamp vs Timestamptz (with timezone)
Imagine we use `timestamptz`. Now 2 users that are haflway around the world from each other can insert data into the database, and the same exact timestamp will be recorded.

Furthermore, the time that appears in our database will actually change if we change the timezone that is set in the pg client:
```sql
set timezone to 'Europe/Paris'
-- ts looks like this: `2021-04-29 17:51:42.316944+02`

set timezone to 'Pacific/Tahiti'
-- ts now looks like this: `2021-04-29 05:54:15.419514-10`
```

Tstz can be thought of a way of abstracting timezones. If I am looking at a set of data, I can directly compare all timezones because of this normalization.
- ex. `2021-04-29 08:51:42.316944-07` and `2021-04-29 08:52:15.419514-07` are the timestamps recorded of 2 users in different timezones, 1 minute apart from each other.

If you manage an application with users in different time zones and you want to display time in their own local preferred time zone, then you can set timezone in your application code before doing any timestamp related processing, and have PostgreSQL do all the hard work for you.

Even when using timestamps with time zone, PostgreSQL will not store the time zone in use at input time, so there’s no way from our tstz table to know that the entries are at the same time but just from different places.

### Functions
###### now()
- returns the timestamp of the current transaction.
	- Therefore, the result is equal if we call `now()` in different parts of the transaction.

###### clock_timestamp()
- returns us the actual timestamp of when the code is being executed.
	- Therefore, if we use this in a transaction, the actual time an operation took place *will* be recorded

# References
[Timezone List](https://www.postgresql.org/docs/8.1/datetime-keywords.html#DATETIME-TIMEZONE-SET-TABLE)
