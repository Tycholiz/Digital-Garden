
Decorating a class with `@Module` provides metadata that Nest makes use of to organize the application structure.

Each application has at least one module, a root module. 
- this is the starting point Nest uses to build the application graph, which is the internal data structure used to resolve module and provider relationships and dependencies.

modules are the way to organize your components in Nest.

For most applications, the resulting architecture will employ multiple modules, each encapsulating a closely related set of capabilities.
- good practice is to have each module correspond to a [[domain|general.principles.DDD]]

The `@Module()` decorator takes a single object whose properties describe the module:
- *providers* - the providers that will be instantiated by the Nest injector and that may be shared at least across this module
- *controllers* - the set of controllers defined in this module which have to be instantiated
- *imports* - the list of imported modules that export the providers which are required in this module
- *exports* - the subset of providers that are provided by this module and should be available in other modules which import this module. You can use either the provider itself or just its token (provide value)

modules are singletons by default, and thus you can share the same instance of any provider between multiple modules effortlessly.
- Every module is automatically a shared module: Once created it can be reused by any module

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
We can make a set of providers global with the `@Global` decorator:
```ts
@Global()
@Module({
  // Module configuration
```


## CLI
- generate a module named "cats" - `nest g module cats`