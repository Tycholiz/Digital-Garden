
A lambda allows us to assign some functionality to a variable.

In Javascript, if we wanted, we could extract out a callback (e.g. `double`) to be its own function:
```js
const double = num => num * 2
const doubled = numbers.map(double)
```

In Ruby, the same way we'd do this is with Lambdas or [[Procs|ruby.lang.proc]]
```rb
double = lambda do |num|
  return num * 2
end
```

Lambdas are essentially [[procs|ruby.lang.proc]] with some distinguishing factors.
- They are more like “regular” methods in two ways:
	1. they enforce the number of arguments (ie. strict arity) passed when they’re called.
	2. they treat the `return` keyword in the same way that a method does. They return as methods (instead of in their parent scope).

Here we encapsulate some logging to a variable:

```rb
say_something = -> { puts "This is a lambda" }
```

For the code inside the lambda to actually run, we need to call it:
```rb
say_something.call
```

### Proc vs Lambda
This function will yield control to the proc, so when it returns, the function returns. Calling the function in this example will never print the output and return 10:
```rb
def return_from_proc
  a = Proc.new { return 10 }.call
  puts "This will never be printed."
end
```

When using a lambda, it will be printed. Calling return in the lambda will behave like calling return in a method, so the `a` variable is populated with 10 and the line is printed to the console.
```rb
def return_from_lambda
  a = lambda { return 10 }.call
  puts "The lambda returned #{a}, and this will be printed."
end
```
