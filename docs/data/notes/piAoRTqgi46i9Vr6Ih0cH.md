
Typescript supports [[overloading|paradigm.oop.overloading]]

In TS, we can overload a function by having multiple call signatures, though it is still the same physical function at runtime (of course, since TS doesn't exist at runtime)
- From the point of view of the caller, this is virtually the same as languages that properly have overloading.

The point of overload signatures is that it allows you to express certain function contracts that can't be safely implemented in TS.
- So almost any overloaded function is going to have some casting, or `any`, or some such 'type unsafeness', otherwise you wouldn't need the overloads

When implementing overloads, you're making things stricter for the caller at the expense of some safety inside the function

In the presence of overloads, the implementation signature is invisible to callers

Example:
Stripe has 2 different versions of `stripe.paymentIntents.list`. The first version looks like this:
```
(params?: PaymentIntentListParams, options?: RequestOptions): ApiListPromise<PaymentIntent>
```

The second looks like this:
```
(options?: RequestOptions): ApiListPromise<PaymentIntent>
```

The benefit to doing this is that we can call the same function, even if we don't want to pass in a `params` object. When we call `stripe.paymentIntents.list`, the TS compiler will figure out which version of the function it should call, based on the arguments that are passed to the function call. This process is what is known as Method overloading.
- If we call `stripe.paymentIntents.list` and our arguments don't line up with the first function signature (ex. because one of the args we pass doesn't line up with `PaymentIntentListParams` interface), then TS will attempt to use the second version of the function. This will result in Intellisense spitting both errors at us, even though only the first one is what we should pay attention to
