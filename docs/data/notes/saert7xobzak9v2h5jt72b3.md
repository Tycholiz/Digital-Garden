
An interceptor is a [[Provider|nestjs.providers]] that makes it possible to
- bind extra logic before / after route handlers are called
  - this is possible because the provided `intercept()` method wraps both the request and response streams.
    - we are able to build custom logic that runs after due to the [[observable|rxjs.objects.observable]] pattern.
- transform the result returned from a function
- transform the exception thrown from a function
- extend the basic function behavior
- completely override a function depending on specific conditions (e.g., for caching purposes)

These capabilities of interceptors are inspired by Aspect Oriented Programming (AOP)
- AOP itself is about adding behavior to existing code without modifying the code itself, allowing us to implement logic like *"log all function calls when the function's name begins with 'set'"*
  - effectively, what we are doing here is injecting some custom logic immediately prior to some handler being called. Thus, we are adding functionlity, but the original code remains unaltered.

an interceptor class is annotated with the `@Injectable()` decorator and implements the `NestInterceptor` interface.

```ts
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  // Because the interceptor class implements the `NestInterceptor` class, our interceptor has access to the `intercept()` method
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');

    const now = Date.now();
    return next
      .handle()
      .pipe(
        // tap() is from RxJS and is used to perform side-effects for notifications from the source observable
        tap(() => console.log(`After... ${Date.now() - now}ms`)),
      );
  }
}
```

### `intercept()`
The `intercept` function returns an [[observable|rxjs.objects.observable]]

Args:
- The `ExecutionContext` provides methods that allow us to query for information about the current execution process.
- `CallHandler` provides access to the response stream
