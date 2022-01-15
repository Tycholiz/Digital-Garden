
the indent level of a function should not be greater than one or two.
- If a function does only those steps that are one level below the stated name of the function, then the function is doing one thing.

### Total vs Partial Functions
Functions can be either total or partial.
- A total function is one that will succeed for all possible inputs, whereas a partial function may not.

note: technically all functions in dynamic languages like Javscript are partial, because the function call will fail if we pass in the wrong type (something not possible in a statically typed language)

#### Total Fn
No matter what numbers you specify, this function will always return a value. You can be confident that under no circumstances will your program crash because of this function (Reality is slightly more complex, but this is close enough).
As such, this function is a total function.

```js
function add( x, y ){ return x + y };

// All numbers can be converted to strings, so this is a total function.
function numberToString( num ) { return num.toString() };
```

#### Partial Fn
This function has the possibility of crashing. If you specify the divisor as zero, you'll get an "Divide by zero" error.  therefore, this function is a partial function.

```js
function divide( number, divisor ) { return number / divisor }

// Not all strings can be converted to numbers, so some inputs to this might fail.
// therefore, this is a partial function.
function stringTonumber( str ){ .... }
```

If a total function calls a partial function, then the total function becomes "infected", and therefore becomes a partial function.
- This makes sense, since now the function call can fail and throw an exception.
- the way we deal with this is by using a `try`/`catch`. We anticipate errors happening, and we give an alternative result. If our fetch for a user returns no results, we want to show a "user not found" notification to the user.
	- using `try`/`catch` blocks allows us to inch a partial function closer towards total function status, since we are reducing the probability of a generic error that breaks the program, or otherwise allows it to function in a state that was not intended.
