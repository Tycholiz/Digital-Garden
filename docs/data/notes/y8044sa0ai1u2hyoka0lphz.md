
Modules are the way to organize your components in Nest.
- Decorating a class with `@Module` provides metadata that Nest makes use of to organize the application structure.

Each application has at least one module, a root module. 
- this is the starting point Nest uses to build the application graph, which is the internal data structure used to resolve module and provider relationships and dependencies.
- this is the module that we register in our `main.ts` with `NestFactory.create(AppModule)`

For most applications, the resulting architecture will employ multiple modules, each encapsulating a closely related set of capabilities.
- good practice is to have each module correspond to a [[domain|general.principles.DDD]]

The module encapsulates providers by default
- This means that in order to inject providers into a different module, they must be exported from the imported module.

modules are singletons by default
- thus you can share the same instance of any provider between multiple modules effortlessly.
- Every module is automatically a shared module: Once created it can be reused by any module
  - to do this, simply add the service to the `exports` array of the shared module. Now, any module that imports the shared module will have access to the service listed in that `exports` array.

![](/assets/images/2023-01-23-12-12-32.png)

```ts
@Module({
  // correspond to the HttpService class that gets injected into the provider (service).
  imports: [HttpModule],
  providers: [AutomationService],
  exports: [AutomationService],
})
export class AutomationModule {
  // Modules can inject providers (e.g. for configuration purposes):
  constructor(private automationService: AutomationService) {}
}
```

Modules themselves cannot be injected due to circular dependency.

## Parts of a module
The `@Module()` decorator takes a single object whose properties describe the module:
- `providers` 
- `controllers`
- `imports`
- `exports`

### `providers`
The `providers` property is an array of providers that will be instantiated by the Nest injector and that may be shared at least across this module. 
- When we put a provider here, we are registering it with the [[IoC container|general.patterns.IoC#ioc-container,1:#*]] (ie. the NestJS runtime) so that it can be instantiated at the site where it is injected (this is dependency injection).

### `controllers`
the set of controllers defined in this module which have to be instantiated

### `imports`
the list of imported modules that export the providers which are required in this module
- put another way, if there is a provider from another module and we want to use it in our module, then we must add the provider to that module's `exports` list, and then include that module in our `imports` list.

this array enables sharing of providers across modules

Don't add the same provider to multiple modules. Instead, export the provider, and import the module.

### `exports`
the subset of providers that are provided by this module and should be available in other modules which import this module. You can use either the provider itself or just its token (provide value)

you may consider the exported providers as the module's API

Modules can export their internal providers
- In addition, they can re-export modules that they import.

## Dynamic Modules
Dynamic modules enable us to easily create customizable modules that can register and configure [[providers|nestjs.providers]] dynamically.

Dynamic modules are created in the module class:
```ts
import { createDatabaseProviders } from './database.providers';
import { Connection } from './connection.provider';

@Module({
  providers: [Connection],
})
export class DatabaseModule {
  // forRoot may be async or sync
  static forRoot(entities = [], options?): DynamicModule {
    const providers = createDatabaseProviders(options, entities);
    return {
      module: DatabaseModule,
      providers: providers,
      exports: providers,
    };
  }
}
```

- [docs](https://docs.nestjs.com/fundamentals/dynamic-modules)

## Global Modules
Global modules are useful for when you want to provide a set of providers which should be available everywhere out-of-the-box (e.g., helpers, database connections, etc.)

We can make a set of providers global with the `@Global` decorator:
```ts
@Global()
@Module({
  // Module configuration
```

## CLI
- generate a module named "cats" - `nest g module cats`