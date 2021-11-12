
`Record<string, number>`

constructs an object type, whose keys are of type `string`, and properties are of type `number`

```ts
const peopleAge: Record<string, number> = {
    'James': 22,
    'Frank': 45
}
```

Of course, we can pass custom types as well:
```ts
interface CatInfo {
  age: number;
  breed: string;
}
 
type CatName = "miffy" | "boris" | "mordred";
 
const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```

What if we wanted to write a function that takes in an array of objects, and those objects could have any shape *as long as they have an id field*?
```ts
function getIds<TElement extends Record<'id', string>>(elements: TElement[]) {
    return elements.map(element => element.id);
}
```