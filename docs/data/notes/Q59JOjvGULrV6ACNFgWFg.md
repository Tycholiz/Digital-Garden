
In Ruby, iterables are arrays, hashes and ranges.

#### `.map` function
```rb
array.map { |string| string.upcase }
```

#### Iterate over hash map
Here, we have arguments to the callback(?) for the key and value.
```
hash = { bacon: "protein", apple: "fruit" }
hash.map { |k,v| [k, v.to_sym] }.to_h
```
