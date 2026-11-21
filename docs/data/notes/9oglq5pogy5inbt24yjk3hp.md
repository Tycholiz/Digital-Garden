
Nestjs middleware is equivalent to [[Express middleware|express.middleware]]

Custom middleware is implemented either in either a function, or in a class with an `@Injectable()` decorator.
- The class should implement the `NestMiddleware` interface, while the function does not have any special requirements.

Middleware fully supports dependency injection.

We set up middleware using the `configure()` method of the module class. 
- Modules that include middleware have to implement the `NestModule` interface.

```ts
@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats');
  }
}
```