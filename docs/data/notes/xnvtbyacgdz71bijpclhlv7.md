
A shim is a piece of code that intercepts API calls transparently (ie. without the user knowing about it) and either:
- changes the arguments that are passed
- handles the operation itself
- redirects the operation elsewhere

Shims are commonly used to support... 
- an old API in a new environment
    - ex. a common occurrence is that shims are released following the behaviour change of a public API. In these cases, a shim can be made that bridges the gap between the old usage of the API (which clients are still using) and the new API.
- a new API in an old environment
    - ex. a polyfill, which is just a shim for a [[browser]] API.
        - when we use experimental language features via a polyfill, there is a shim in the background that intercepts our usage of the feature and changes it in some way so that it can be understood by the language engine.