
Date objects are represented internally by a timestamp, which is milliseconds passed since the epoch (ie. midnight 01 January, 1970 UTC)

There are always two ways to interpret a timestamp: 
1. as a local time 
    - The local timezone is not stored in the date object, but is determined by the host environment.
2. as a Coordinated Universal Time (UTC)

### String format
The JavaScript specification only specifies one string format to be universally supported: `YYYY-MM-DDTHH:mm:ss.sssZ` 
- `T` is a literal character, which indicates the beginning of the time part of the string
- `Z` is the timezone offset, which can either be the literal character `Z` (indicating UTC), or + or - followed by HH:mm, the offset in hours and minutes from UTC.
    - When the time zone offset is absent, date-only forms are interpreted as a UTC time and date-time forms are interpreted as local time (see below for date-only and date-time form explanation)

Various components can be omitted, so the following are all valid:
- *Date-only form*: `YYYY`, `YYYY-MM`, `YYYY-MM-DD`
- *Date-time form*: one of the above date-only forms, followed by `T`, followed by `HH:mm`, `HH:mm:ss`, or `HH:mm:ss.sss`. Each combination can be followed by a time zone offset.

#### Converting a Date object to string
- `Date.toISOString()` will return a string in the format: `1970-01-01T00:00:00.000Z` 
    - note: this is the same string format that the Date object accepts as input
- `Date.toString()` will return a string in the format `Thu Jan 01 1970 00:00:00 GMT+0000`
- `Date.toDateString()` and `Date.toTimeString()` return the date and time parts of the string, respectively.
- `Date.toLocaleDateString()`, `Date.toLocaleTimeString()`, and `Date.toLocaleString()` use locale-specific date and time formats, usually provided by the Intl API.

* * *

months are zero-based, so December is 11

### `Date.now()` vs `new Date()`
`new Date()` - creates a Date object representing the current (or passed) date/time
`Date.now()` - returns the number of milliseconds since the epoch 
- we also get this with `new Date().valueOf()` and `new Date().getTime()`

As a general rule of thumb, use... 
- `Date.now()` in code that deals with intervals (usually subtracting an earlier date from a later date to calculate the time elapsed) 
- `new Date()` for timestamps, e.g. those written to a database.
