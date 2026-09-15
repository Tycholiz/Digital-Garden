
The `Test` class is useful for providing an application execution context that essentially mocks the full Nest runtime, but gives you hooks that make it easy to manage class instances, including mocking and overriding.

### `createTestingModule`
`Test.createTestingModule()` takes a module metadata object as its argument. It returns a `TestingModule` instance which in turn provides a few methods. 
- to this method we pass an object that looks the same as when we are defining a `@Module` in a `.module.ts` file.
  - we are really just trying to simulate the module in a minimalist way, where we include only what is needed for the particular test file.
- the resulting `TestingModule` is an isolated NestJS runtime. As a result, it gets all the NestJS behaviors like dependency injection.
  - for unit tests, `compile()` is the most important one, which bootstraps a module with its dependencies (similar to the way an application is bootstrapped in the conventional `main.ts` file using `NestFactory.create()`), and returns a module that is ready for testing.

The resultant TestingModule is limited to what you define when using the Test class (e.g. which `providers`/`controllers` you include)
- ex. if you pass `TweetsService` to `providers`, then you will get access to all of the methods inside `TweetsService`.
  - you will want to make an instance of an injectable/controller:
  ```ts
  service = module.get<TweetsService>(TweetsService);
  ```

### Mocking
Imagine we have the following TestingModule:
```ts
const module: TestingModule = await Test.createTestingModule({
  controllers: [UsersController],
  providers: [UsersService]
}).compile()
```

If `UsersService` has dependencies that need to be injected in order for it to work, we will get the `Nest can't resolve dependencies` error. If we don't care about the dependencies of the `UsersService`, we can simply mock it:
```ts
let controller: UsersController
const mockUsersService = {}
beforeEach(() => {
  const module: TestingModule = await Test.createTestingModule({
    controllers: [UsersController],
    providers: [UsersService]
  })
    .overrideProvider(UsersService)
    .useValue(mockUsersService)
    .compile()

  service = module.get<TweetsService>(TweetsService);
})
```