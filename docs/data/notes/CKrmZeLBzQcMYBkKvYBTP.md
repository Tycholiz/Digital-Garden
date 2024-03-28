
`Pick<Type, Keys>`
- Constructs a type by picking the set of properties `Keys` (string literal or union of string literals) from `Type`.

In the below example, we are creating a new type `TypePreview` based on a subset of fields available on the `Todo` type.
```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```
