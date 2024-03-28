
This is the object design pattern by which objects are assessed based on their ability to respond to particular methods
- In other words, if it responds to the methods you want, there's no reason to be particular about the type.

```rb
# Say bye to everybody
def say_bye
  if @names.nil?
    puts "..."
  elsif @names.respond_to?("join")
    # Join the list elements with commas
    puts "Goodbye #{@names.join(", ")}.  Come back soon!"
  else
    puts "Goodbye #{@names}.  Come back soon!"
  end
end
```
The `say_bye` method doesn’t use each, instead it checks to see if `@names` responds to the join method, and if so, uses it. Otherwise, it just prints out the variable as a string. This method of not caring about the actual type of a variable, just relying on what methods it supports is known as “Duck Typing”, as in “if it walks like a duck and quacks like a duck…”. The benefit of this is that it doesn’t unnecessarily restrict the types of variables that are supported. If someone comes up with a new kind of list class, as long as it implements the join method with the same semantics as other lists, everything will work as planned.
