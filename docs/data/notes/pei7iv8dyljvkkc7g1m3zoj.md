
a declarative programming paradigm concerned with [[data streams|general.arch.streaming]] and the propagation of change.

Reactive programming is well-suited for implementing interactive user interfaces and real-time system animation.
- It's really no coincidence that modern front-end frameworks like React, Svelte, Vue etc. all implement this reactive type of paradigm.
  - ex. in React, if a piece of state changes, the React engine internally understands the dependency tree of that value. Because of this, it knows when to re-render components, because its dependencies have changed.
- Microsoft Excel is another program that implements the reactive paradigm. Consider that a calculation in a single cell has dependencies. When the value of those dependencies change, it must propogate to the cell to do a recalculation.

Any component of a system that exists beyond a network call (e.g. a backend database) cannot effectively be a part of this reactive loop, since we can't realistically keep queries updated in real-time.
- instead database interactions are modeled as side-effects which must interact with the reactive system.

### Example
In most programming languages, `a = b + c` will result a value being assigned to `a`. Later on if `b` or `c` changes, it has no impact on the value of `a`, since that value has already been declared.

In reactive programming, this is not the case. Any future changes to `b` or `c` will cause `a` to "react" and update. Therefore, `a` has dependencies of `b` and `c`, and this is registered somewhere. As a result, the language runtime knows based on this dependency graph when recalculations should be made.