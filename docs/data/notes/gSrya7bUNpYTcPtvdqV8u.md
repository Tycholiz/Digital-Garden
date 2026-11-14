
### Factorial function
Implementing a factorial is a good starting-point for coming to grips with recursion. It is a simple implementation, and the logic lends itself well to a recursive solution.

```js
function factorial(num) {
    if (num === 1) return 1
    return num * factorial(num - 1)
}
```

Given `factorial(5)`, each "iteration" looks like this:
1. return 5 * factorial(4)
2. return 4 * factorial(3)
3. return 3 * factorial(2)
4. return 2 * factorial(1)
5. return 1

Consider that when we see `factorial(num - 1)`, we really don't know what this value is going to be for each iteration until the very end. This is part of what makes it mentally challenging to parse. If we work backwards and plug in the return value of each step into the return value of the previous step's call to `factorial()`, then it's more clear how the value from the final "iteration" bubbles up.


### Reverse a string
We start from the end of the string. We take the last character of the string string[string.length - 1] and call the function reverseString() again, but with the last character removed. When this child function returns, this string[string.length - 1] will be appended at the beginning of the returned string.
```js
function reverseString(string) {
  // Base case
  if (string === "") {
    return string;
  }

  // Recursive case
  else {
    // take the last character and add it to the reverse of the previous characters
    return string[string.length - 1] + reverseString(string.substr(0, string.length - 1));
  }
}
```

### Simulate reduce with recursion
each call of the function separates the first argument from the rest, takes something from that first argument, then calling the function again with the remaining arguments (all but first)
```js
function sum(num1,...nums) {
    if (nums.length == 0) return num1;
    return num1 + sum( ...nums );
}
```
```js
function maxEven(num1,...restNums) {
    var maxRest = restNums.length > 0 ?
            maxEven( ...restNums ) :
            undefined;

    return (num1 % 2 != 0 || num1 < maxRest) ?
        maxRest :
        num1;
}
```
