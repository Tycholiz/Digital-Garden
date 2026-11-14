
Attribute accessors provide us with a streamlined way to define getters and setters for instance variables. Otherwise, they are left private.

Let’s say that you have a class with instance variables and you want to expose them to the outside world:

```rb
class Food
  def initialize(protein)
		# this instance variable cannot be accessed from outside
    @protein = protein
  end
end

bacon = Food.new(25)
bacon.protein # NoMethodError: undefined method `protein'
```

To solve this, we'd define a getter and setter:
```rb
class Food
	# getter
	def protein
		@protein
	end
	# setter
	def protein=(value)
		@protein = value
	end
end
```

Lucky for us, Ruby provides attribute accessors so we don't have to do this by hand.

```rb
class Food
	attr_accessor :protein, :calories
	def initialize(protein, calories)
		@protein = protein
		@calories = calories
	end
end
```

Above, we are having 4 new methods defined for us:
1. `protein`
2. `protein=`
3. `calories`
4. `calories=`

* * *

- `attr_reader` - generate *getter*
- `attr_writer` - generate *setter*
- `attr_accessor` - generate *getter* and *setter*
