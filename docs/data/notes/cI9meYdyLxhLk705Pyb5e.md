
### Run a typescript file
```sh
# the respawn flag will cause the file to be recompiled upon making changes
node-dev file.ts --respawn
```

### Destructure obj/array with types
Intuitively, we might try and do:
```ts
const { name: string, age: number } = body.value
```

But in reality we are renaming the destructed values. The correct way is like this:
```ts
const { name, age }: { name: string; age: number } = body.value
```
