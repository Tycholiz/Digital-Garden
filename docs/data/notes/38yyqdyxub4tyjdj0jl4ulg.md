
A provider is a class that can be injected into the constructor of other classes (dependency injection)

A class is marked as a `provider` by using the `@Injectable()` decorator.

The main idea of a provider is that it can be injected as dependency; this means objects can create various relationships with each other, and the function of "wiring up" instances of objects can largely be delegated to the Nest runtime system.

A provider "provides" some value to the dependent that it is being used in. 
- a service is a provider that, when injected into a class, allows the class to use the methods defined on the provider itself.
  - ex. we could have a `NuggetsService` provider, whose role is to fetch nuggets. Now, from other providers we can inject the `NuggetService`, allowing us to fetch nuggets from that other provider.

Providers normally have a lifetime ("scope") synchronized with the application lifecycle. 
- When the application is bootstrapped, every dependency must be resolved, and therefore every provider has to be instantiated. 
- Similarly, when the application shuts down, each provider will be destroyed. 
  - there are ways to make your provider lifetime request-scoped as well.

Providers are plain JavaScript classes that are declared as providers in a module.

Responsible for things like:
- data storage and retrieval (Service)
- repositories, factories, helpers

Nest has a built-in [[IoC container|general.patterns.IoC#ioc-container,1:#*]] that resolves relationships between providers.

The `@Injectable()` decorator attaches metadata, which declares that the class can be managed by the Nest IoC container

### Registration process
Registration happens in the `module` file.

Registration is about associating a token (name/id) with a class.

```ts
// app.module.ts

// this is shorthand
providers: [CatsService],

// for this
providers: [
  {
    provide: CatsService, // token
    useClass: CatsService,
  },
];
```
Seeing the explicit construction helps to see how the registration process (into the Nestjs IoC container) is really just a mapping between the token and the class.
- This is done for convenience to simplify the most common use-case, where the token is used to request an instance of a class by the same name.

### Injecting non-service based Providers
Though the most common use is to inject services, we can really inject any kind of value, allowing us to do things like put an external library into the Nest container, or replace a real implementation with a mock object (useful for testing).
- ex. here, we are associating a string-valued token (`'CONNECTION'`) with a pre-existing connection object we've imported from an external file:
```ts
providers: [
  {
    provide: 'CONNECTION',
    useValue: connection,
  },
],
```

Which can be used in a provider:
```ts
@Injectable()
export class CatsRepository {
  constructor(@Inject('CONNECTION') connection: Connection) {}
}
```

### Asynchronous providers
At times, the application start should be delayed until one or more asynchronous tasks are completed. 
- ex. you may not want to start accepting requests until the connection with the database has been established. 

This can be accomplished by registering a provider as an `async` function along the `useFactory` syntax.
```ts
providers: [
  {
    provide: 'ASYNC_CONNECTION',
    useFactory: async () => {
      const connection = await createConnection(options);
      return connection;
    },
  }
]
```

Which can be injected into a class like any other provider with `@Inject('ASYNC_CONNECTION')`

Here, Nest will await resolution of the `useFactory` promise before instantiating any class that depends on (injects) such a provider.

