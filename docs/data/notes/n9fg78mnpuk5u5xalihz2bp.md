
A Guard is a [[provider|nestjs.providers]] that enables us to implement [[authorization|auth]]
- in traditional [[express]] apps this is usually handled by middleware.
  - the main difference is that middleware is dumb, not knowing which handler will get called after it calls `next()`. On the other hand Guards have access to the `ExecutionContext` instance. Therefore, they know exactly what will be executed next.

Guards are invoked before [[pipes|nestjs.pipe]] or [[interceptors|nestjs.interceptor]]

A guard is created by making a class that extends the `CanActivate` interface and is annotated with the `@Injectable()` decorator.

Guards determine (at run-time) whether a given request will be handled by the route handler.
- ex. they can check conditions such as permissions, roles, ACLs, etc. to make that determination.

