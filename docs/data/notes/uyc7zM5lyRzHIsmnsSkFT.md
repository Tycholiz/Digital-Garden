
An iterator gives us the ability to say "get me the next item in the list". The iterator is what hands us the next item in the sequence.
- Therefore, the iterator manages the state of which element of the list we are currently on (known as the `Head`). Because of this `Head`, an iterator is fully aware of its location.
	- More complex iterators may let us move forwards and backwards, but they all must satisfy this simple requirement of knowing where it is.
- The type of list is immaterial (ie. could be an array, linked list, object etc.)
