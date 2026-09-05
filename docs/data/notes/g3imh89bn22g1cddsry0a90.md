

In Go a pointer is represented using the `*` character followed by the type of the stored value.

`*` is also used to "dereference" pointer variables. Dereferencing a pointer gives us access to the value the pointer points to. 

When we write `*xPtr = 0` we are saying "store the int 0 in the memory location that `xPtr` refers to". 
- If we try `xPtr = 0` instead we will get a compiler error because `xPtr` is not an `int` it's a `*int`, which can only be given another `*int`.

We use the `&` operator to find the address of a variable. `&x` returns a `*int` (pointer to an `int`) because `x` is an `int`. This is what allows us to modify the original variable. `&x` in main and `xPtr` in zero refer to the same memory location.

When we use the `&` operator in front of a variable `c`, we are talking about the place where `c` is stored. When we use the `*` operator, we are talking about the variable `c` itself.

We can get the memory address of a variable by prepending it with an `&`
```go
i := 7
fmt.Println(&i) // 0xc420014090
```

As expected, the `inc` function wouldn't cause the value of `i` to change, since `i` is copied by value, so `x` in the `inc` function is just a copy, which gets incremented, but then is discarded, since the function doesn't return anything.
```go
func main() {
  i := 7
  inc(i)
  fmt.Println(i) // 7
}

func inc(x int) {
  x++
}
```

If we want to modify the value of `i` here, then we can do so by passing a pointer to the variable to `inc`. In this case, the `inc` function receives a value at that memory reference and is able to modify the original version.
```go
func main() {
  i := 7
  // pass the pointer of `i` to the `inc` function
  inc(&i)
  fmt.Println(i) // 7
}

// here, we say "the function accepts a pointer that, when dereferenced, is an integer"
func inc(x *int) {
  // dereference the pointer before incrementing; otherwise we'd be incrementing the memory address (ie. 0xc420014090).
  *x++
}
```

### Use-cases
Pointers give us the ability to define a function that accepts a pointer as an argument so that the original variable can be modified from within the function.
```go
func zero(xPtr *int) {
  *xPtr = 0
}
func main() {
  x := 5
  zero(&x)
  fmt.Println(x) // x is 0
}
```
- this also has the added benefit of not having to copy the value into a local function version on each method call.

Pointers are extremely useful when paired with structs.