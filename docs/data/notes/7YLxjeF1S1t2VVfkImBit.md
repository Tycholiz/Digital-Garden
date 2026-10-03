
Lazy means that the algorithm is only doing the work when it needs to; it is not acting ahead of time.

with eager evaluation we get the whole list right away, but we have to remember the whole thing too. for the lazy evaluation we create only the smallest part of the list that we care about. we are lazy. we don't evaluate the whole thing, we just get as much of it as you ask for. do as little work as possible.

### Example: addition
Imagine you have a program that has to solve the math problem: 1 + 2 + 3 + 4 + 5

A lazy algorithm would take the first 2 numbers, add them, then ask for the next number and add it to the total, and repeat until there are no more numbers.
An eager algorithm would take all the numbers at once and add them at the same time.
