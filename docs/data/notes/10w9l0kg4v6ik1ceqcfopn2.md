
## Dependencies
Say we are making a simple app with an HTTP route (to provide post data). Our app consists of a controller (HTTP endpoint), a provider (logic to access the data) and main module.
1. In our controller, we declare the PostsService as a dependency in the constructor
  - PostsService is available to be injected in the first place because it has the `@Injectable` decorator.
2. In PostsModule, the class `PostsService` is associated with a token also called `PostsService` (`providers` key)
  - the token is used by the Nestjs runtime to request an instance of a class (most often by the same name. This is configurable, and is what we are doing when we provide the long-form object with keys `provide` and `useFactory`/`useValue`, `useClass`)
```ts
@Module({
  controllers: [PostsController],
  providers: [PostsService],
})
```