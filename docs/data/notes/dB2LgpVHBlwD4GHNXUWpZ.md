
# Concepts
### Type assignability
We say that type A is assignable to type B if you can use a value of type A in all places where a value of type B is required. For example:
- assigning a value to a variable
- passing a function argument
- returning a value from a function that has an explicit return type

### Optionality
consider that by default, `null` and `undefined` are part of the [[domain|dendron://thoughts-on/math.domain]] of virtually every type. That is, a string can be `null`, an object property can be `null`, and so on. When we enable strict null checking, we are removing `null` from the domains of these types
![](/assets/images/2021-10-21-15-22-40.png)
- `strictNullChecks` forces you to explicitly distinguish between values that can be `null` or `undefined` and those that cannot be.

we could explicitly extend the type to once again include `null` as part of the type's domain:
```ts
function bar(person: Person | null) {
```

But this may not be preferable. Instead we can use optional params (in functions and interfaces) to determine if something can be optional or not:
- also beneficial to use [[optional chaining|js.lang.op.optional-chaining]] to achieve the effect of optionality, (spec:)as opposed to type guards
```ts
interface Person {}

function hello(person?: Person) {}

interface Employee {
  id: number;
  name: string;
  supervisorId?: number;
}

hello();
const employee: Employee = {
  id: 1,
  name: 'John',
};
/* * * * */
interface Person { hello(): void }

function sayHello(person: Person | undefined) {
  person.hello(); // 🔴 Error!

    // this is a type guard, and it allows the Typescript Type checker to 
    // deduce that the type of person inside the if statement is narrowed to just `Person`
    if (person) {
        person.hello(); // OK
    }
}
```

Consider that a `getUser` function might not return a user for a given ID. Therefore, we should set the return value of the function as a union between User and undefined.
```ts
getUser(id: number): User | undefined {
  return this.users[id];
}
```

* * *

spec: If we don't want to specify a new type, we can just do this:
```ts
type TodoPrevious = Pick<Todo, "title" | "completed">
```

Here, `"title" | "completed"` resolves to a type. We could have written it like this:
```ts
type ValueKeys = "tite" | "completed"
type TodoPrevious = Pick<Todo, ValueKeys>
```
