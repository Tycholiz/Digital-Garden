
Windows split the infinite input stream into "buckets" of finite size.

a window is created as soon as the first element that should belong to this window arrives

the window is completely removed when the time (event or processing time) passes its end timestamp plus the user-specified allowed lateness (see Allowed Lateness). 
- ex. for an *event-time-based window* that creates non-overlapping (or tumbling) windows every 5 minutes and has an allowed lateness of 1 min, Flink will create a new window for the interval between 12:00 and 12:05 when the first element with a timestamp that falls into this interval arrives, and it will remove it when the watermark passes the 12:06 timestamp.
- removal only guaranteed for *time-based windows*

### WindowAssigner
The window assigner is responsible for determining where each incoming element is assigned to a window.
- we specify the type of WindowAssigner to use in the call to `window(...)` (for keyed streams) or the `windowAll()` (for non-keyed streams)

Flink has pre-defined window assigners for the most common use cases (*tumbling windows*, *sliding windows*, *session windows* and *global windows*).
- All pre-defined window assigners (except the global windows) assign elements to windows based on time (which can either be processing time or event time).
- [[see: explanation on window types|general.patterns.streaming.window]]

```java
// tumbling event-time windows
input
  .keyBy(<key selector>)
  .window(TumblingEventTimeWindows.of(Time.seconds(5)))
  .<windowed transformation>(<window function>);
```

### Trigger
A Trigger determines when a window (as formed by the window assigner) is ready to be processed by the window function. Each `WindowAssigner` comes with a default Trigger. 
- a custom trigger can also be specified if necessary using `trigger(...)`.

The trigger interface has five methods that allow a Trigger to react to different events:
- The `onElement()` method is called for each element that is added to a window.
- The `onEventTime()` method is called when a registered event-time timer fires.
- The `onProcessingTime()` method is called when a registered processing-time timer fires.
- The `onMerge()` method is relevant for stateful triggers and merges the states of two triggers when their corresponding windows merge, e.g. when using session windows.
- Finally the `clear()` method performs any action needed upon removal of the corresponding window.

### Evictor
The evictor has the ability to remove elements from a window after the trigger fires and before and/or after the window function is applied. The two methods to do this are:
- `void evictBefore(Iterable<TimestampedValue<T>> elements, int size, W window, EvictorContext evictorContext)`
- `void evictAfter(Iterable<TimestampedValue<T>> elements, int size, W window, EvictorContext evictorContext)`

Flink comes with three pre-implemented evictors. These are:
1. `CountEvictor`: keeps up to a user-specified number of elements from the window and discards the remaining ones from the beginning of the window buffer.
2. `DeltaEvictor`: takes a DeltaFunction and a threshold, computes the delta between the last element in the window buffer and each of the remaining ones, and removes the ones with a delta greater or equal to the threshold.
3. `TimeEvictor`: takes as argument an interval in milliseconds and for a given window, it finds the maximum timestamp max_ts among its elements and removes all the elements with timestamps smaller than max_ts - interval.

### Window Function
The *window function* is responsible for processing the elements (ie. performing some computation) of each window.
- therefore, the trigger determines when the window function gets called.

The *window function* can be one of `ReduceFunction`, `AggregateFunction`, or `ProcessWindowFunction`. 
- ReduceFunction specifies how two elements from the input are combined to produce an output element of the same type.
- AggregateFunction is a generalized version of a ReduceFunction that has three types: an input type (IN), accumulator type (ACC), and an output type (OUT)

```java
// ReduceFunction
input
  .keyBy(<key selector>)
  .window(<window assigner>)
  .reduce(new ReduceFunction<Tuple2<String, Long>>() {
    public Tuple2<String, Long> reduce(Tuple2<String, Long> v1, Tuple2<String, Long> v2) {
      return new Tuple2<>(v1.f0, v1.f1 + v2.f1);
    }
  });
```

### Window Join
A *window join* joins the elements of two streams that share a common key and lie in the same window.
- these windows are defined by the *window assigner*

The elements from both streams are passed to a user-defined `JoinFunction` or `FlatJoinFunction` where the user can emit results that meet the join criteria.

```java
stream.join(otherStream)
  .where(<KeySelector>)
  .equalTo(<KeySelector>)
  .window(<WindowAssigner>)
  .apply(<JoinFunction>);
```

#### Types of Window Joins
- Tumbling Window Join
- Sliding Window Join
- Session Window Join
- Interval Join

[docs](https://nightlies.apache.org/flink/flink-docs-master/docs/dev/datastream/operators/joining/)
