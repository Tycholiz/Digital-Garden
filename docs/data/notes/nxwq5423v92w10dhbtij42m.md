
### Declaring variables
```go
var x int = 7
// or the equivalent short-form
x := 7
```

### If statements
```go
if x > 6 {
  fmt.Println("yup, bigger than 6")
} else if x < 2 {
  fmt.Println("nope, smaller than 2")
}
```

### `defer`
A `defer` statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

### Type assertions
```go
var i interface{} = "hello"

s, ok := i.(string)
// If i holds a string, then `s` will be the underlying value and `ok` will be true. Otherwise `ok` will be false
```

This statement asserts that the interface value i holds the concrete type T and assigns the underlying T value to the variable t.
- If i does not hold a T, the statement will trigger a panic.

### Error handling
Go doesn't have exceptions. This absense is relieved by the fact that functions can return multiple values. To get the error, we set one of our return values to the error and check for its existence after running the function.
```go
result, error := getSomeData() 
if error {
  return fmt.Errorf("no data available")
}
```
