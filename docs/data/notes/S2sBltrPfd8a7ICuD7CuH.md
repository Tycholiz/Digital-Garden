
The whole value of typescript in being able to define code that is type-safe, and have any non-type safe code to be uncovered *at* compile time. This inevitably saves us a large amount of effort, since we don't have to wait until runtime to be able to catch these bugs. It makes our code tighter, easier to reason about and more robust.
- Generics is one way to implement this when writing functions

TypeScript’s type system is very powerful because it allows expressing types in terms of other types. We can accomplish this via:
- [[generics|ts.lang.generic]]
- [[typeof operator|ts.lang.op.typeof]]
- [[keyof operator|ts.lang.op.keyof]]
- indexed access types
- conditional types
- mapped types
- template literal types

Naturally, any code that does not exist as far as Javascript is concerned (e.g. Generics, Interfaces) does not add to the runtime cost of the code.



#### non-null assertion
Non-null assertion operator shouldn't be used unless you have a very good reason to do so
- Non-null assertion operators are simply a way of telling the compiler, "trust me, I’m absolutely sure that this optional value is in fact never empty."
- While you might think that a value is always non-empty, you could be wrong. Or worse, you could be correct at the moment, but it will not be the case in the future, because some other developer will call your function with a null or undefined.
```ts
function sayHello(person: Person | undefined) {
    person!.hello(); // no errors!
}
```

The operator could come in handy, but generally our feeling for it should be that if we are reaching for it, there is probably a more correct way to do it.
- sometimes the overhead of the type system is not justified, and we just want to create a quick escape hatch that makes sense. We have to address these case by case.

Another situation in which it might make sense to use this operator is when you want to enable strict null checks on a large, existing codebase. Enabling the flag in such a case might result in hundreds or thousands of errors. It’s a huge effort to go through them all at once. On the other hand, you cannot merge your changes to the master if the whole project doesn’t compile successfully. In this case, the non-null assertion operator might come in handy, as it will allow you to quickly suppress the errors that you don’t have time to fix properly.

### Compiler
We can use ESBuild to compile typescript on server side
