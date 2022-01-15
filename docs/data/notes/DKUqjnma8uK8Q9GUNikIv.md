
The ES6 `sorted` method takes in a `comparator` callback.

### Comparator callback
each array element is sorted according to the return value of the callback.
- `undefined` values are just popped onto the end

If we omit the comparator callback, then the sort order is ascending/alphabetical (ie. as a string). Therefore, an array of numbers becomes an array of strings, then the strings are compared alphabetically, making `[1, 10, 100, 3, 5, 7, 9]` perfectly sorted.

If possible, prefer to use Lodash's `sortBy`, as it provides more sensible defaults
