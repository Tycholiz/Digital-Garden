
The array argument passed to callbackFn is most useful if you want to read another index during iteration, because you may not always have an existing variable that refers to the current array. You should generally not mutate the array during iteration (see mutating initial array in iterative methods), but you can also use this argument to do so. The array argument is not the array that is being built, in the case of methods like map(), filter(), and flatMap() — there is no way to access the array being built from the callback function.

Operating on arrays offers `O(1)` search speed if the index is known, but adding/removing an element is slow since size of array cannot change once it's created
- note: is this relevant for Javascript?

`forEach` is almost always used for side-effects at the end of a chain
