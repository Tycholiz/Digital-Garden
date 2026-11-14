
A block is an anonymous code block that can be passed into a method. Therefore, a block is nothing more than a snippet code that can be created now, but executed later. Blocks are passed to methods that `yield` them within with the `do` and `end` keywords.
- From within that, every time `yield` is called, that code block that was passed in will get executed
- ex. The block is passed to the each method of an array object.

```rb
def someMethod
  hello = 'hello'
  yield(hello) if block_given?
end

# the block is between the curly braces
someMethod { |h| h }
```

Something implemented in JS with callbacks would be implemented with blocks in Ruby:
- Blocks -> Ruby
- Callbacks -> Javascript

Blocks are essentially methods, since they:
- can accept arguments
- returns a value

But, they:
- have no name
- do not belong to an object

a block is a [[closure|general.lang.feat.closure]], so it can refer to variables from the scope in which it was declared.

Blocks can only be created by passing them to a method when the method is called.

Blocks are snippets of code that can be created to be executed later.

Blocks are used extensively in Ruby for passing bits of code to functions. By using the `yield` keyword, a block can be implicitly passed without having to convert it to a proc.

If we are using the `.each` method, we are using blocks. The part between `do` and `end` is the block:
- the `|name|` is the parameter of the block
```rb
@names.each do |name|
  puts "Hello #{name}!"
end
```

Internally, the `each` method will essentially call `yield "Albert"`, then `yield "Brenda"` and then `yield "Charles"`, and so on.

Conceptually, this is what's happening under the hood:
```rb
def each
  i = 0
  while i < size
    yield at(i)
    i += 1
  end
end
```

The real power of blocks is when dealing with things that are more complicated than lists. Beyond handling simple housekeeping details within the method, you can also handle setup, teardown, and errors

## Two ways of calling Blocks
Blocks can be called in one of 2 ways
1. by using `yield` (implicit blocks)
2. by capturing it by specifying the final argument of the method as `&block` and using `block.call` (explicit blocks)

### `yield` and Implicit Blocks
In Ruby, methods can take blocks implicitly and explicitly
- Implicit block passing works by calling the `yield` keyword in a method.
  - `yield` finds and calls a passed block, so you don’t have to add the block to the list of arguments the method accepts.

Because Ruby allows implicit block passing, you can call all methods with a block. If it doesn’t call `yield`, the block is ignored. If the called method does `yield`, the passed block is found and called with any arguments that were passed to the `yield` keyword.

### Explicitly accepting blocks
We can explicitly accept a block in a method by adding it as an argument using an ampersand parameter (usually called `&block`). Since the block is now explicit, we can use the `#call` method directly on the resulting object instead of relying on `yield`.
- The `&block` argument is not a proper argument, so calling this method with anything else than a block will produce an `ArgumentError`.

```rb
def each_explicit(&block)
  return to_enum(:each) unless block

  i = 0
  while i < size
    block.call at(i)
    i += 1
  end
end
```

When a block is passed like this and stored in a variable, it is automatically converted to a *proc*.

It's important to understand the different uses of `&` here as a prefix to the last argument of a function:
- in a function definition, it captures any passed block into that object
- in a function call, it expands the given callback object into a block

### Blocks allow us to implement [[Inversion of Control|general.patterns.IoC]]
By accepting a block from you as a programmer, the method can pass control to you.

### Syntax
The following are identical:
```rb
5.times do |name|
  puts "Oh, hello #{name}!"
end

5.times { |name| puts "Oh, hello #{name}!" }
```
