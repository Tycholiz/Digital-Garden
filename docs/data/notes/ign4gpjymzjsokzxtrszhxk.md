
Here, we declare the `isAdult` method on the `*Person` type.
```go
func (p *Person) isAdult bool {
  return p.Age > 18
}
```


In Go, Methods are functions that have a special *receiver argument*:
```go
// The Abs() method a receiver argument of type Vertex named v.
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

The receiver argument can be either a *value receiver* or a *pointer receiver*
- *value receiver* - makes a copy of the type and passes it to the function so that it has its own local version.
	- Therefore, the original object will remain unchanged.
	- ex. `(v Vertex)` 
- *pointer receiver* - passes the address of a type to the function. The function stack has a reference to the original object. 
	- Therefore, any modifications on the passed object will modify the original object.
	- ex. `(v *Vertex)`

Since methods often need to modify their receiver, pointer receivers (ie. `*x`) are more common than value receivers.

### Pointer Receiver
In this case the receiver argument is a [[pointer|golang.syntax.pointer]], as denoted by `*T`:
```go
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```
- in this case, the `Scale` method has as a receiver argument `*Vertex`.
- now, we can modify the value to which the receiver points

methods with pointer receivers take either a value or a pointer as the receiver when they are called
- this means that you can pass either the value or the pointer to the value (ie. either `v` or `&v`)
- this is a convenience offered by the Go engine. Under the hood, Go interprets the statement `v.Scale(5)` as `(&v).Scale(5)` since the Scale method has a pointer receiver.
```go
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```

In general, all methods on a given type should have either value or pointer receivers, but not a mixture of both.