
- `splice()` changes the original array 
- `slice()` doesn't change the original array.

- `splice()` returns the items that were removed from the array (often, this is simply discarded in favor of the effect of changing the original array via mutation)
- `slice()` returns a new subarray that is constructed by using the original as a reference (therefore immutable)
