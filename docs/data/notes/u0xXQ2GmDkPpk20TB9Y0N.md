
# Hashing 
let's say we have a function that takes a number from zero through nine, adds three and, if the result is greater than ten, subtracts ten. So f(2) = 5, f(8) = 1, etc. Now, we can make another function, call it f', that goes backwards, by adding seven instead of three. f'(5) = 2, f'(1) = 8, etc.
- Theoretically, any mathematical functions that maps one thing to another can be reversed. In practice, though, you can make a function that scrambles its input so well that it's incredibly difficult to reverse.
- taking an input and applying a one-way function is called "hashing" the input
- SHA1 is an example of this kind of "one-way" function
