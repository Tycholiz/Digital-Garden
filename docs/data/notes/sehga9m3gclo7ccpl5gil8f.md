
Slices are an abstraction over arrays to make them easier to work with, most notably because they do not have a fixed length.
```go
var a []int{5, 4, 3, 2, 1}
```

In particular, a slice is a reference to a segment of an array.

With slices we can do things like append (which doesn't modify the original Slice, but returns a new one)
- under the hood, Go is creating a new array, copying the contents from the old one, and adding the new value.