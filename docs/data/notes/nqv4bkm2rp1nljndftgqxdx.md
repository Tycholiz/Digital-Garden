
Controllers are responsible for handling incoming requests and returning responses to the client.
- Controllers should only handle HTTP requests; delegating more complex tasks to providers

Most often, providers are injected into controllers, allowing the controller the ability to call methods on the instantiated provider
- to do this, both the controller and the provider must be registered in the module

The routing mechanism controls which controller receives which requests.

Normally, each controller has more than one route, and different routes can perform different actions.
- in a REST api, `/customers` and `/products` would be 2 different controllers
  - ex. route path prefix implemented as `@Controller('customers')`

The decorators (e.g. `@Controller`, `@Get`) associate classes with required metadata and enable Nest to create a routing map (which associates which requests belong with which controllers)

Route handlers can either return a [[Promise|js.lang.promises]] or an [[observable|rxjs.objects.observable]] stream:
```ts
// Promise approach
@Get()
async findAll(): Promise<any[]> {
  return [];
}

// Observable approach
@Get()
findAll(): Observable<any[]> {
  return of([]);
}
```

If returning an observable stream, Nest will automatically subscribe to the source underneath and take the last emitted value (once the stream is completed)