
A pipe is a [[provider|nestjs.providers]] that operates on the arguments passed to a [[controller|nestjs.controllers]] route handler.
- just before the handler is invoked, Nest will interpose a pipe and it will receive the arguments destined for the handler, allowing it to operate on them.
- when the pipe has finished its operation, it returns control to the handler, which takes in those (potentially) transformed arguments.

Pipes have two typical use cases:
1. transformation: transform input data to the desired form (e.g., from string to integer)
2. validation: evaluate input data and if valid, simply pass it through unchanged; otherwise, throw an exception when the data is incorrect

Nestjs comes with built-in pipes, for use-cases such as:
- parsing a UUID
- parsing an array

### Usage
A pipe is created by making a class that extends the `PipeTransform<T, R>` interface and is annotated with the `@Injectable()` decorator.
- `T` - type of the input value
- `R` - return type of the `transform()` method

Every pipe must implement the `transform()` method to fulfill the `PipeTransform` interface contract.
- `transform(value, metadata)`

To use a pipe, we need to bind an instance of the pipe class to the appropriate context.
- ex. we might want to bind a `ParseIntPipe` with a particular route handler to make sure it runs before the handler method is called.

```ts
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

This code ensures that only one of two things will happen:
1. either the parameter we receive in the `findOne()` method is a `number` (as expected in our call to `this.catsService.findOne()`), 
2. or an exception is thrown before the route handler is called. 

As with other providers in Nestjs, we pass the class rather than an instantiation of the class, leaving the responsibility of instantiation up to the framework. This is to enable [[dependency injection|general.patterns.dependency-injection]].
- alternatively, we can pass an in-place instance (`new ParseIntPipe(options)`) if we wish to pass options to customize the built-in pipe's behaviour.

Nestjs supports both sync and async pipes.
- therefore the `transform()` method may use `async/await`