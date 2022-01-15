
### Enumerables vs Enumerators
#### Enumerable
"Enumerable" defines an object that is meant to be iterated over, passing over each element once in order.
- arrays and objects are examples of *enumerables*
- an Enumerable is an object like an array, list, or any other sort of collection that implements the `IEnumerable` interface
- Enumerables standardize looping over collections, and enables the use of useful extension methods like `List.Where()` or `List.Select()`.

`IEnumerable` is an interface that defines one method `GetEnumerator` which returns an `IEnumerator` interface, this in turn allows readonly access to a collection. A collection that implements `IEnumerable` can be used with a foreach statement.

#### Enumerator
"Enumerator" is an object that can return each item in a collection. Therefore, the enumerator knows the order of items and keeps track of where it is in sequence. It then returns the current item when it is requested.

`IEnumerator` provides two methods, `MoveNext()` and `Reset()`. It also has a property `Current`. 
- `MoveNext` is the method used to step over each item, applying any kind of custom iterator logic in the process
- `Current` is a method used to get the current item after `MoveNext` is done. You end up with an interface that defines objects that can be enumerated, and how to handle that enumeration.

![](/assets/images/2021-08-24-12-52-32.png)
in the above example:

- It gets the object’s enumerator by calling its GetEnumerator method.
- It requests each item from the enumerator and makes it available to your code as the iteration variable, which your code can read

every time you write a foreach loop, you’re using enumerators. You actually can’t use foreach on a collection that doesn’t implement `IEnumerable`. When it gets compiled, a foreach loop like this:

```cs
foreach (Int element in list)
{
    // your code
}
```

Gets turned into a while loop, that processes until the Enumerable is out of items, calling MoveNext each time and setting the iterator variable to .Current.

```cs
IEnumerator enumerator= list.GetEnumerator();
while (list.MoveNext())
{
    element = (Int)enumerator.Current
    // your code
}
```

[resource](https://www.csharpstar.com/difference-between-ienumerator-and-ienumerable-interface-csharp/)

### Coroutine
Coroutines are special functions that can pause execution and return back to the main thread.
- Coroutines return an enumerator
    - therefore, Coroutines can use functions that have a return type of `IEnumerator`
- They’re commonly used for executing long actions that can take some time to finish, without causing the application to hang while waiting on the routine. 
    - For example, in games where the framerate of the application matters a lot, large hitches even on the order of a few milliseconds would hurt the user experience.

to break up execution over different frames, you’ll just need to `yield return null` whenever you’d like to pause execution and run more on the main thread.
- to use this Enumerator, you’ll need to call the function and assign it to a variable, which you can call `MoveNext()` on at regular intervals.

#### in Unity
the Coroutine controller in Unity handles the processing of coroutines for us. All it's really doing is just calling `MoveNext()` once per frame, checking if it can process more, and handling the return value if it’s not null.
