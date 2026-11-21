
Type assertions helps you to force types when you are not in control of them. For e.g. -
1. you are processing user data (with unreliable types)
2. working with data that has changed shape over years (employee code was numeric, now it is alphanumeric :))
3. receiving data from an external program

Type assertions let the Typescript compiler know that a given variable should be treated as belonging to a certain type. There are no “exceptions” or data restructuring associated with assertions, except minimal validations (we refer this behaviour as “validations that are applied statically”).

There are two ways to do type assertions.
1. Bracket syntax. e.g. `let length: number = (<string>lengthField);`
2. Use as. e.g. `let length: number = lengthField as string;`
There is no difference between the two ways and the end result is always the same. Note that if you are using JSX you have to use `as` syntax.

## Cook
### Assert as a subtype on an Interface/Type
```ts
interface Data {
  events: {
    id: string;
  }[];
}
const copiedEvents = [...data.events] as Data['events']
```