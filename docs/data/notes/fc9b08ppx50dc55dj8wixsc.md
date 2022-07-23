
RxJS operators are inspired by array methods (map, filter, reduce, every, etc).

Operators are pure functions that enable a functional programming style of dealing with collections with operations

Operators are the bread and butter of RxJS. They are what allow us to take complex async code and easily compose it in a declarative manner.

There are 2 types of operator:
1. pipeable operators
2. creation operators

## Pipeable Operators
operators that can be piped to observables using the `.pipe(operator)` syntax.
- ex. `filter`, `mergeMap`.
- these operators return a new Observable, whose subscription logic is based on the first Observable.
- since pipeable operators are just functions, we could technically just use them like ordinary functions: `op()(obs)`, but since normally we have multiple operators combined together, they would very quickly become unreadable. This is the purpose of `pipe()`.
  - compare `op4()(op3()(op2()(op1()(obs))))` to `obs.pipe(op1(), op2(), op3(), op4())`

## Creation Operators
operators that can be called as standalone functions to create a new Observable.
- ex. `of(1, 2, 3)` creates an observable that will emit 1, 2 and 3, one right after another.

Their purpose is to create an Observable with some common predefined behavior or by joining other Observables.
- ex. the `interval` function takes a number and produces an Observable.

[creation operator list](https://rxjs.dev/guide/operators#creation-operators-list)