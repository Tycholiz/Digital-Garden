
#### Object where keys are integers as string
```ts
interface IDictionary<TValue> {
    [id: string]: TValue;
}

// later
const myObject: IDictionary<string> = { "0": "teleport" }
```

### Object where keys are not known ahead of time
[source](https://stackoverflow.com/questions/12710905/how-do-i-dynamically-assign-properties-to-an-object-in-typescript#answer-44441178)
```ts
const constants: Record<string, any> = {}

constants.dendronTechUrl = 'https://tycholiz.github.io/Digital-Garden/'

export default constants
```