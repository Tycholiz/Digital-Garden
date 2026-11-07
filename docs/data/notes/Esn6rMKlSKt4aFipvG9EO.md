
#### Define a class
```rb
class Greeter
	def initialize(name = "world")
		@name = name
	end

	def say_hi
		puts "hello #{@name}"
	end
end

myGreeter = Greeter.new("Kyle")
myGreeter.say_hi
```

#### Get instance methods of a class
```rb
# See all
Greeter.instance_methods
# See non-inherited
Greeter.instance_methods(false)
```

#### Check if an object has a particular method on it
```rb
myGreeter.respond_to?('say_hi')
```
