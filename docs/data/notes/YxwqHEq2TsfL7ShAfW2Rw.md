
### What is it and what is it for?
Some computations can have their performance improved by splitting the task up evenly among many computers.
- ex. if we have 100 million numbers and need to find the largest one, we can either give it to one computer to do it, or we can break it into parts (say 100 different groups), and give each group to a different computer. Each computer then solves the problem of finding the largest number among 1 million numbers, and then of the resulting set of 100 numbers, we find the largest one. Doing it this way (ie. in parallel) is exponentially faster.
- problems such as these are called [Embarrassingly Parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel)
    - the method of breaking it down (mapping) into pieces and then joining the individual results to form a global result (reducing) is called *MapReduce*

Hadoop is an open source software that makes doing [[MapReduce|general.patterns.map-reduce]] type programming easier. You dont have to worry about installing the program on your 100 machines, breaking your initial data into pieces, copying it to all 100 machines, copying results over from 100 machines, etc. All the housekeeping is managed by Hadoop. Once you setup a hadoop cluster over the 100 machines, you can give it any program and data and it takes care of all the behind the scenes work and give you back the result.

Hadoop has often been used for implementing [[ETL|db.strategies.etl]] processes
- data from transaction processing systems is dumped into the distributed filesystem in some raw form, and then MapReduce jobs are written to clean up that data, transform it into a relational form, and import it into an MPP data warehouse for analytic purposes

The biggest limitation of [[unix]] tools is that they run only on a single machine—and that’s where tools like Hadoop come in.
