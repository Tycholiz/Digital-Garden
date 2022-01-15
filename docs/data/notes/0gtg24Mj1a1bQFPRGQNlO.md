
Everything in Ruby is an object, and therefore has a class that it's derived from.

### Object Types in Ruby
Specifying what type any particular object is in Ruby is a bit of a wobbly concept. Since Ruby takes a [[duck typing|general.terms.duck-typing]] approach, it's not so important what object it is, but rather what it can do.

Therefore, it's not common in Ruby-land to want to check the type of a particular object. Instead, it makes more sense to test its ability to respond to certain methods with `object.respond_to?(:to_s)`.

### Data types
- Numbers
- Boolean
- Strings
- Hashes
- Arrays
- Symbols

We can verify what type the object is with `response.instance_of?(Array)`

### Loose syntax rules of Ruby
Ruby allows you to omit parenthesis `()` and in some cases curly braces `{}`, sometimes making the code harder to read:
```rb
# the following
has_many :models, dependent: :destroy

# is identical to:
has_many(:models, { dependent: :destroy } )
# has_many takes in 2 args; a symbol and a hashmap
```




* * *

### Built-in variables
- `__FILE__` - name of the current file
- `$0` - name of the file that was originally executed

Ruby treats `?` and `!` as actual characters in a method name. respond_to and respond_to? are different. `?` indicates a boolean evaluation (by convention; not strict requirement).

* * *

### Closures
Ruby doesn't have [[first-class functions|general.lang.feat.functions.first-class]], but it does have closures in the form of [[blocks|ruby.lang.block]] , procs and [[lambdas|ruby.lang.lambda]]
