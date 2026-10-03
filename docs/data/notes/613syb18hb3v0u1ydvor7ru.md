
Nestjs is a server-side application framework
- highly opinionated
- supports multiple data paradigms, such as ClientServer, [[pub/sub|general.patterns.messaging.pubsub]] etc.
- achieves SOLID principles by the use of [[modules|nestjs.modules]].
- module dependency is achieved with [[constructor dependency injection|general.patterns.dependency-injection#nestjs-constructor-dependency-injection]]. 
  - Modules import [[providers|nestjs.providers]] to perform some purpose (such as retrieve data from a database, send a message etc.), and those providers are made available within the module. To pass
- Nestjs provides an all-tools-supplied approach. 
  - ex. It has modules out of the box for connections to most types of databases/datastreams (e.g. RDBs, Redis, Kafka, RabbitMQ) and 

## Platform
Nestjs is built on top of an HTTP Node framework
- default options are [[Express|express]] and Fastify

Depending on which underlying platform we use, we will get access to its API (`NestExpressApplication` or `NestFastifyApplication`)
- the resulting `app` object will have access to the methods of whichever underlying platform we choose.
```ts
const app = await NestFactory.create<NestExpressApplication>(AppModule);
```

in Nest, almost everything is shared across incoming requests. 
- ex. We have a connection pool to the database, singleton services with global state, etc.

By default, modules are eagerly loaded (which means that as soon as the application loads, so do all the modules, whether or not they are immediately necessary).
- this is fine for the most part, but if we are running in a [[serverless|deploy.serverless]] environment, this would be problematic.
  - in this case we want to lazy load the modules.

![](/assets/images/2022-05-16-13-27-38.png)

### Lifecycle
A Nest application, as well as every application element, has a lifecycle managed by Nest. 
- Nest provides lifecycle hooks that give visibility into key lifecycle events, and the ability to run registered code in your `module`, `injectable` or `controller` when they occur.

Lifecycle events can be broken into 3 phases: *initializing*, *running* and *terminating*

### Dependency Injection and Decorators
Nest is built around the pattern of [[dependency injection|general.patterns.dependency-injection]], which is achieved through [[decorators|js.lang.decorator]].
- This is mostly done through the constructor (therefore, it's constructor-based dependency injection)

The general pattern is that we create a [[provider|nestjs.providers]] by decorating a class with `@Injectable`, then we pass that class into another c
- the `@Injectable` decorator enables the class to be registered and managed by the [[IoC|general.patterns.IoC]] container (`@Modules`)

When we inject a class into another class, Nest will instantiate the class and pass it to the controller's constructor.

Because of Typescript in Nestjs, it is very easy to manage dependencies, since they are resolved by type.

### Context
Guards, filters and interceptors are meant to be generic. That is, they don't care if they are being used in an HTTP context or WebSockets context.
- Nestjs provides utility classes that provide information about the current execution context.
  - this information can be used to build generic [[guards|nestjs.guard]], filters, and [[interceptors|nestjs.interceptor]] that can work across a broad set of [[controllers|nestjs.controllers]], methods, and execution contexts.
- Two of these utility classes are `ArgumentsHost` and `ExecutionContext`.

#### `ArgumentsHost`
`ArgumentsHost` is simply an abstraction over a handler's arguments.
- ex. for HTTP server applications, the `host` object encapsulates Express's `[request, response, next]` array
- ex. for Graphql server applications, the `host` object contains the `[root, args, context, info]` array.

This class provides methods for retrieving the arguments being passed to a handler. With it, we choose the appropriate context, and then we retrieve the arguments.

##### Methods
- `getType()` tells us which context is being used (`http`, `rpc` etc.)
- `getArgs()` gets us back an array of arguments being passed to the handler
  - this is generic, since we don't know what arguments we are getting back (since the context has not been specified). For this reason, using `switchTo...` methods might be preferable, since they return more specific types (such as `HttpArgumentsHost` or `RpcArgumentsHost`)
- `switchToHttp()`
```ts
const ctx = host.switchToHttp();
const request = ctx.getRequest<Request>();
```

#### `ExecutionContext`
`ExecutionContext` extends `ArgumentsHost`, and gives us additional details about the current execution process.

Nest provides an instance of `ExecutionContext` in places you may need it, such as in the `canActivate()` method of a guard and the `intercept()` method of an interceptor. 

##### Methods
- `getClass()` Returns the type of the controller class which the current handler belongs to.
- `getHandler()` Returns a reference to the handler (method) that will be invoked next in the request pipeline.
  - ex. in an HTTP context, if the currently processed request is a POST request, bound to the `create()` method on the `CatsController`, `getHandler()` returns a reference to the `create()` method and `getClass()` returns the `CatsController` type (not instance).

The ability to access these references gives us the opportunity to access the metadata set through the `@SetMetadata()` decorator from within guards or interceptors.

### Metadata and Reflection
- [docs](https://docs.nestjs.com/fundamentals/execution-context#reflection-and-metadata)

We can attach custom metadata to route handlers with the `@SetMetadata()` decorator, which can then be accessed from within the class via dependency injection (with the `Reflector` helper class) to make certain decisions.
- ex. we can create some metadata about the roles that a user has.


* * *

## Core Components
### Modules
![[nestjs.modules]]

### Controllers
![[nestjs.controllers]]

### Providers
![[nestjs.providers]]

### Middleware
![[nestjs.middleware]]

## Misc
- In Nest, a microservice is defined as an application that uses a different transport layer than HTTP.