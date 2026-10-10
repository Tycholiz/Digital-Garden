
### regular for-loop
```go
for i := 0; i < 5; i++ {
  fmt.Println("hey")
}
```

### while-loop
```go
i := 0
for i < 5 {
  fmt.Println(i)
  i++
}
```

### loop over an array (using `range`)
```go
arr := []string{"a", "b", "c"}

for index, value := range arr {
  fmt.Println("index:", index, "value:", value)
}


var myMap = make(map[string]int)
for key, value := range arr {
  fmt.Println("key:", key, "value:", value)
}
```