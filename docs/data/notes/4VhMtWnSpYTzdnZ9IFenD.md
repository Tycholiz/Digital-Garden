
#### Alias a module
Imagine we had a function `term`, but we want to create an alias `regex`, so that either could be imported and used:
```ts
export const term = () => {
    // do stuff
}
export { term as regex }
```
