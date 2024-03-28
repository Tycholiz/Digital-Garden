
Doesn't work like [[express middleware|express.middleware]]
- instead, inspired by koa.js

A middleware function takes 2 args:
- resolver data (ie. `root`, `args`, `context`, `info`)
- the `next` function, which is used to control execution of the next middleware in the stack, as well as the resolver to which the middleware is attached.
  - the way this differs from Express middleware is that this `next` function returns a Promise that resolves to the value returned by the subsequent middleware and resolver in the stack.

Because of how `next` works, it is simple to perform actions before and/or after resolver execution:
```ts
export const ResolveTime: MiddlewareFn = async ({ info }, next) => {
  // before
  const start = Date.now();
  await next();
  // after
  const resolveTime = Date.now() - start;
  console.log(`${info.parentType.name}.${info.fieldName} [${resolveTime} ms]`);
};
```

If we only want to do something before an action, like log the access to the resolver, we can just place the return next() statement at the end of our middleware:
```ts
const LogAccess: MiddlewareFn<TContext> = ({ context, info }, next) => {
  const username: string = context.username || "guest";
  console.log(`Logging access: ${username} -> ${info.parentType.name}.${info.fieldName}`);
  return next();
};
```

### Guards
Guarding is a technique to programmatically break the middleware stack (which is done by purposefully not calling `next()`)
- in doing this, the result returned from the middleware will be used instead of calling the resolver and returning its result.
- We can also throw an error in the middleware if the execution must be terminated and an error returned to the user, e.g. when resolver arguments are incorrect.

It's called a *guard* because it prevents execution of the resolver and prevents any data return.
