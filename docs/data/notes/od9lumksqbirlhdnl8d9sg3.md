
Key-value storage, where:
- All values can be of different type.
- Need to know all the different fields at compile time.
- Keys don’t support indexing, (spec: so they can't be iterated over)

```go
// define the struct type
type person struct {
  name string
  age int

}

func main() {
  // instantiate a person
  p := person{name: "Kyle", age: 30}
  fmt.Println(p.name) // Kyle
}
```