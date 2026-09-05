
```go
// a function that takes in 2 ints and returns an int
func sum(x int, y int) int {
  return x + y
}
```

In Go, functions can have multiple return values. If we choose to write our functions with 2 return vlaues, then we must be sure to *always* return all of them.
- notice below that in the error case, we are still returning `0` as our first return value.
```go
import "errors", "math"

func sqrt(x float64) (float64, error) {
  // logic: square root of a negative number throws an error
  if x < 0 {
    return 0, errors.New("Cannot get square root of negative number")
  }
  return math.Sqrt(x), nil
}

sqrtOf16, err := sqrt(16)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(sqrtOf16)
}
```

In Go function, if we specify the result parameters (ie. return value(s)), then we can just specify `return` without any value/expression list following it.
- the result parameters act as ordinary local variables. They can be modified, and we can `return` (again, with nothing after) and our function will return those same result parameters:

```go
// short-form
func a()(x float64, err error){
    x = 7.0
    // `return` here will return bother a and b, 
    // since that is what is specified above in the result parameters
    return
}
// long-form
func a() (float64,error){
  x := 7.0
  var err error
  return x,err
}
```
