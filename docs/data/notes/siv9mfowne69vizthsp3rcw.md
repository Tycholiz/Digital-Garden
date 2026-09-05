
A pool is a collection of resources that are kept, in memory, ready to use, rather than the memory acquired on use and the memory released afterwards.
- In this context, resources can refer to system resources such as file handles, which are external to a process, or internal resources such as objects.
- A pool client requests a resource from the pool and performs desired operations on the returned resource. When the client finishes its use of the resource, it is returned to the pool rather than released and lost.

The pooling of resources can offer a significant response-time boost in situations that have high cost associated with resource acquiring, high rate of the requests for resources, and a low overall count of simultaneously used resources.

Pooling is also useful when the latency is a concern, because a pool offers predictable times required to obtain resources since they have already been acquired. 

Pooling is also useful for expensive-to-compute data, notably large graphic objects like fonts or bitmaps, acting essentially as a data cache or a memoization technique.

Special cases of pools are [[connection pools|db.strategies.pool]], thread pools, and memory pools.

## Object Pool
A pool (a.k.a *object pool*) is used to manage object caching. 
- A client can access the pool which will hold objects that have already been instantiated, thus negating the client's need to instantiate a new object itself.
- The pool is often self-managing, and will instantiate new objects as they are needed, and limit the creation of more to adhere to a pre-defined limit.
- When a client is done with the object, it is returned to the pool to be used by another client.

The pool itself is designed to be a singleton (ie. there is only a single instance of it)

```ts
class ObjectPool<T> {
  private objects: T[]
  public acquire() {
    // take an object from the pool and give it to the client
  }
  public release() {
    // return an object from the client back to the pool
  }
}
```

## UE Resources
- https://en.wikipedia.org/wiki/Pool_(computer_science)