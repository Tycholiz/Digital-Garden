
Apache Flink is a stream processing framework that is scalable and fault-tolerant.
- it is based on the idea that it should not be hard to express simple computations (like calculating an average and then grouping by a certain attribute) while still be able to scale indefinitely, and in a fault-tolerant manner.
- Flink is [[MapReduce|general.patterns.map-reduce]] like [[apache.hadoop]], but with streaming data.

Flink is a dataflow engine.
- Like MapReduce, they work by repeatedly calling a user-defined function to process one record at a time on a single thread. They parallelize work by partitioning inputs, and they copy the output of one func‐ tion over the network to become the input to another function.
- Unlike in MapReduce, these functions need not take the strict roles of alternating map and reduce, but instead can be assembled in more flexible ways (these functions are called operators)

Flink will basically partition the incoming stream of data and then perform some computation on that subset of data.
- Flink allows us to specify how much parallelism you want for each of these subtasks. To scale up is to simply increase the parallelism of the bottleneck subtask.

### Example: Stock market aggregator
Problem statement: Imagine we have a program that consumes a stream of stock market trades and we want to get data on how many trades happen by industry each minute
- this sort of problem has scalability problems built-in, since we can expect there to be a large amount of data, and we will have no forewarning of when trade volume unexpectedly increases.

Implementation: Configure Flink to:
- partition the input stream based on the industry name of the stock
- apply a moving average on a window of 1 minute.

Consider that reading from the stream and applying an `AVG` calculation are 2 different tasks. Flink allows us to scale up each independently. Therefore, if we determine that trade volume has increased and there are more trades happening per minute, we simply scale up the number of readers. On the other hand, if the number of industries to group by has increased, we simply scale up the number of operators that calculate the `AVG`.

## E Resources
- https://medium.com/archsaber/a-simple-introduction-to-apache-flink-2a603119041e