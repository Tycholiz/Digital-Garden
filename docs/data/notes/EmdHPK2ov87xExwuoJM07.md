
A Partial type is derived from an existing type. The only difference between the Partial type and the original type is that the Partial type has all properties set to optional.
- note: if you want a type that only includes some of the fields from another type, then you're looking for [[ts.type.util.pick]]

Example:
```ts
interface Todo {
	importance: number;
	text: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return {
		...todo,
		...fieldsToUpdate,
	};
}
```

Above, the resulting Partial type would be like this:
```ts
interface PartialTodo {
	importance?: number,
	text?: string,
}
