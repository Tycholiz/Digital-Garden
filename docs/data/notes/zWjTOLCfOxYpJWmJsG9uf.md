
[[See|general.lang.feat.symbol]]

Symbols are defined by prepending a variable with `:`.

Symbols have the distinct feature that any two symbols named the same will be identical:
```rb
"foo".equal? "foo"  # false
:foo.equal? :foo    # true
```

This makes comparing two symbols really fast (since only a pointer comparison is involved, as opposed to comparing all the characters like you would in a string)

Keys in a hash are implemented as symbols. Hashes were originally implemented in Ruby like this:
```rb
hash = {
   :key => "value",
   :another_key => 4
}
```

But the syntax was simplified in v1.9:
```rb
hash = {
   key: "value",
   another_key: 4
}
```

### Should you use String or Symbol?
No clear answer exists, only guidelines:
- if the text at hand is "data", then use a string;if it's "code", then use a symbol, especially when used as keys in hashes
- symbols aren't really text, even though they read well. Instead, they are unique identifiers, like numbers, or bar codes. While strings represent data that can change, symbols represent unique values, which are static.
- if you use strings that contain the same text in your code multiple times, then a new string object will be created every time.
   - ex. `puts "hello"` 10 times will create 10 string objects.
