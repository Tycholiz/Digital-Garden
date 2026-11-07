
## Operators
Operators transform one or more `DataStreams` into a new `DataStream`.

### `map`/`flatMap`
suitable only when performing a one-to-one transformation: for each and every stream element coming in, `map()` will emit one transformed element
- Otherwise, you will want to use `flatmap()`

### `keyBy`
Logically partitions a stream.
- All records with the same key are assigned to the same partition.

It is often very useful to be able to partition a stream around one of its attributes
- the result is that all events with the same value of that attribute are grouped together.
- ex. imagine we had a stream of data for taxi rides, and we wanted to find the longest taxi rides starting in each of the grid cells.
  - if this were a SQL query, this would mean doing some sort of `GROUP BY` with the startCell, while in Flink this is done with keyBy(KeySelector)