
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