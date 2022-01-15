
### Date.now() vs new Date()
`new Date()` - creates a Date object representing the current date/time
`Date.now()` - returns the number of milliseconds since midnight 01 January, 1970 UTC
- we also get this with `new Date().valueOf()` and `new Date().getTime()`

As a general rule of thumb, you may find it clearer to use `Date.now()` in code that deals with intervals (usually subtracting an earlier date from a later date to calculate the time elapsed), and `new Date()` for timestamps, e.g. those written to a database.
