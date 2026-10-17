
#### Check if a variable is iterable
Here, we basically check if there is an `each` method on the `@names` instance variable
```rb
if @names.respond_to?("each")
```

#### Access shell variables
```rb
require 'pathname'

Pathname.new(ENV['HOME'])
```
