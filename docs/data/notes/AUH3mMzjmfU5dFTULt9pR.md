
A closure is a [[first-class function|general.lang.feat.functions.first-class]] with an environment.
- The environment is a mapping to the variables that existed when the closure was created. The closure will retain its access to these variables, even if they’re defined in another scope.

A closure is a function that encloses its surrounding state by referencing fields external to its body. The enclosed state remains across invocations of the closure.
- in the `inner`/`outer` function illustration, the `inner` function is the closure. The fact that it has access to the state of the `outer` function's scope is key to its quality of being a closure.

A closure occurs when the function we are calling returns another function, so the second function lives on, while the initial function dies (because it has already returned). Put another way, a closure is formed any time an inner function is made available from outside of the scope it was defined within.

Normally, when you exit a function, all its variables “disappear”. This is because nothing needs them anymore. But what if you declare a function inside a function? Then the inner function could still be called later, and read the variables of the outer function. In practice, this is very useful! But for this to work, the outer function’s variables need to “stick around” somewhere. So in this case, JavaScript takes care of “keeping the variables alive” instead of “forgetting” them as it would usually do.

# UE Resources
[Recommended resource: Closures in Ruby](https://reprog.wordpress.com/2010/02/27/closures-finally-explained/)
