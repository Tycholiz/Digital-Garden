
This is similar to the `any` type, but is safer because it’s not legal to do anything with an `unknown` value

If a type is of type `unknown`, then it means the variable can be assigned to any type. At the moment of initializing an `unknown` variable, we are saying "we don't know the type of this one. If you know it, feel tree to coerce it into its proper type. You can change it however you want."

On the other hand, it also means that we can't set the `unknown` value to a different type.
```ts
// This is ok
let x: unknown;

x = 123; // no error
/* * * * * * * * * * */
// This is not
function foo2(bar: unknown) {
  const a: string = bar // error: Type 'unknown' is not assignable to type 'string'
  //...
}
```

Both `any` and `unknown` represent an unknown type, but there is a key difference:
- as demonstrated in the code above, all types are assignable to the `unknown` type, but `unknown` is not assignable to any type
    - therefore `unknown` is a type-safe alternative to `any`

![](/assets/images/2021-10-21-08-39-26.png)

This distinction between `any` and `unknown` is important, because it makes our code stricter. 
- If you have a value of `unknown` type, you need to cast it to some other type before you can do anything useful with it. The consequence of this is that `unknown` doesn’t propagate like any does, which is much safer.

## Use-cases
When you are working with external values (e.g. APIs), there is a good chance you won't know what the return type is. In such cases, it’s a good idea to type such value as `unknown` instead of `any`.
```ts
interface Article {
    title: string;
    body: string;
}

fetch("http://example.com")
    .then(response => response.json())
    .then((body: unknown) => {
        const article = body as Article;
        // we need to cast, otherwise we'd get an error
        console.log(article.title);
    });
```

In this example, we use type assertion to tell TypeScript that we know the type of body. It’s still not type-safe because we could be wrong, but at least it’s explicit. The proper solution here would be to perform a runtime check to make sure that body is indeed an Article. We’ll look into such a solution in the lesson dedicated to user-defined type guards.
