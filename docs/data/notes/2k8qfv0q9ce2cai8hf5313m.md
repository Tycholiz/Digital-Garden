
MapReduce is a concept that solves problems by applying a two-step process: the map phase and the reduce phase.
- The map phase looks at all documents separately one after the other and creates a map result.
- The reduce phase takes in the results from the map phase and reduces them to produce an end result.

MapReduce is an abstraction on top of a distributed filesystem.

implementing a complex processing job using the raw MapReduce APIs is actually quite hard and laborious
- various higher-level programming models (Pig, Hive, Cascading, Crunch) were created as abstractions on top of MapReduce.

MapReduce is a [[batch|general.patterns.batching]] processing algorithm.

Each MapReduce job is independent from every other job.

MapReduce is very robust: you can use it to process almost arbitrarily large quantities of data on an unreliable multi-tenant system with frequent task terminations, and it will still get the job done (albeit slowly). On the other hand, other tools are sometimes orders of magnitude faster for some kinds of processing.

MapReduce is designed to tolerate frequent unexpected task termination, making it more appropriate for larger jobs— jobs that process so much data and run for such a long time that they are likely to experience at least one task failure along the way.

MapReduce has no concept of [[indexes|db.strategies.index]], at least not in the usual sense.
- this is because there is usually no need to find individual items— we normally use MapReduce to aggregate data in some way.

Using the MapReduce programming model has separated the physical network communication aspects of the computation (getting the data to the right machine) from the application logic (processing the data once you have it). This separation contrasts with the typical use of databases, where a request to fetch data from a database often occurs somewhere deep inside a piece of application code
- Since MapReduce handles all network communication, it also shields the application code from having to worry about partial failures, such as the crash of another node: MapReduce transparently retries failed tasks without affecting the application logic.

MapReduce is a bit like [[unix]] tools, but distributed across potentially thousands of machines. 
- Like Unix tools, it is a fairly blunt, brute-force, but surprisingly effective tool. 
- A single MapReduce job is comparable to a single Unix process: it takes one or more inputs and produces one or more outputs.
- While Unix tools use stdin and stdout as input and output, MapReduce jobs read and write files on a distributed filesystem.
  - ex. In Hadoop’s implementation of MapReduce, that filesystem is called HDFS (Hadoop Distributed File System). HDFS conceptually creates one big filesystem that can use the space on the disks of all machines running the daemon.
- Unlike pipelined Unix commands, MapReduce can parallelize a computation across many machines, without you having to write code to explicitly handle the parallelism.
  - The mapper and reducer only operate on one record at a time; they don’t need to know where their input is coming from or their output is going to, so the framework can handle the complexities of moving data between machines.

it is very common for MapReduce jobs to be chained together into workflows, such that the output of one job becomes the input to the next job.
- ex. Workflows consisting of 50 to 100 MapReduce jobs are common when building recommendation systems

The pattern of data processing in MapReduce is very similar to this example:
1. Break files into records: Read a set of input files, and break it up into records. 
  - In the web server log example, each record is one line in the log (that is, `\n` is the record separator).
2. Map: Call the mapper function to extract a key and value from each input record. 
  - In the preceding example, the mapper function is `awk '{print $7}'`: it extracts the URL ($7) as the key, and leaves the value empty.
3. Sort: Sort all of the key-value pairs by key. 
  - In the log example, this is done by the first sort command.
  - sorting is implicit, because the results from map are sorted before they are given to the reduce function.
4. Reduce: Call the reducer function to iterate over the sorted key-value pairs. If there are multiple occurrences of the same key, the sorting has made them adjacent in the list, so it is easy to combine those values without having to keep a lot of state in memory. 
  - In the preceding example, the reducer is implemented by the command `uniq -c`, which counts the number of adjacent records with the same key.

### Mapper
The mapper is called once for every input record
- job is to extract the key and value from the input record. For each input, it may generate any number of key-value pairs (including none). 
- It does not keep any state from one input record to the next, so each record is handled independently.

### Reducer
The MapReduce framework takes the key-value pairs produced by the mappers, collects all the values belonging to the same key, and calls the reducer with an iterator over that collection of values. The reducer can produce output records (such as the number of occurrences of the same URL).

One way of looking at this architecture is that mappers "send messages" to the reducers. When a mapper emits a key-value pair, the key acts like the destination address to which the value should be delivered. Even though the key is just an arbitrary string (not an actual network address like an IP address and port number), it behaves like an address: all key-value pairs with the same key will be delivered to the same destination (a call to the reducer).


# UE Resources
https://timilearning.com/posts/mit-6.824/lecture-1-mapreduce/
