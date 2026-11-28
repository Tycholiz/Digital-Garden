
### Ambient Declaration
The `declare` keyword is used to tell Typescript "this thing (usually a variable) exists already, and can therefore be referenced by other code".
- Also, it tells the Typescript compiler that there is no need to compile this statement into Javascript

Imagine our project calls some third party JS script which returns an object. Since Javascript has no typings, we would have no typing information on this object. `declare` gives us a mechanism through which we can say "trust me, this variable exists and has *this* type". The compiler will use this statement to statically check other code but will not trans-compile it into any JavaScript in the output.
- this enables our Typescript to be able to call, say, a method on that returned object, since we've typed it with the `declare` keyword.