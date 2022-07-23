
Generic interfaces are very much like functions that transform types.

A generic interface lets us define a function signature, and enforce that signature on a future value that we define. Put another way, we are describing a generic function
```ts
interface GenericIdentityFn {
  <Type>(arg: Type): Type;
}
 
function identity<Type>(arg: Type): Type {
  return arg;
}
 
let myIdentity: GenericIdentityFn = identity;
```

In this case, when we use `GenericIdentityFn` we now will also need to specify the corresponding type argument (here: `number`), effectively locking in what the underlying call signature will use. This changes its use markedly from the previous implementation.
```ts
interface GenericIdentityFn<Type> {
  (arg: Type): Type;
}
 
function identity<Type>(arg: Type): Type {
  return arg;
}
 
let myIdentity: GenericIdentityFn<number> = identity;
```

This changes it slightly. Now we have a non-generic function signature that is a part of the generic type
- Understanding when to put the type parameter directly on the call signature and when to put it on the interface itself will be helpful in describing what aspects of a type are generic.

It’s not possible to refer to a generic interface without providing its type argument.

For example, if you want to write a function that accepts an instance of a generic interface, you have two options:
1. Write a specialized function that accepts a very specific instance of the interface (e.g. `FormField<string>`)
2. Write a generic function
```ts
interface FormField<T> {
    value?: T;
    defaultValue: T;
    isValid: boolean;
}

// Very specialized. Only works with `FormField<string>`.
function getStringFieldValue(field: FormField<string>): string {
    if (!field.isValid || field.value === undefined) {
        // Thanks to the specialization, the compiler knows the exact type of `field.defaultValue`.
        return field.defaultValue.toLowerCase();
    }
    return field.value;
}

// Generic. Can be called with any `FormField`.
// It’s important to understand that here, T is the type argument of the function, not of the FormField interface. It gets passed only to FormField like any regular type does.
function getFieldValue<T>(field: FormField<T>): T {
    if (!field.isValid || field.value === undefined) {
        // On the other hand, we don't know anything about the type of `field.defaultValue`.
        return field.defaultValue;
    }
    return field.value;
}
```

### Generic constraints on interfaces (Type argument constraint)
you might decide to enforce the fact that `FormField` should only contain string, number, or boolean values.
```ts
interface FormField<T extends string | number | boolean> {
    value?: T;
    defaultValue: T;
    isValid: boolean;
}

// Type 'T' does not satisfy the constraint 'string | number | boolean'
function getFieldValue<T>(field: FormField<T>): T { /* ... */ }
```

The reason for this error is that the `T` type argument of the function has no restrictions. You’re trying to pass it to `FormField` which only accepts types that extend string, number or boolean. Therefore, you get a compile error. To get rid of the error, you need to put the same or stricter restrictions on the type argument `T`.

```ts
interface FormField<T extends string | number | boolean> {
    value?: T;
    defaultValue: T;
    isValid: boolean;
}

// Type 'T' does not satisfy the constraint 'string | number | boolean'
function getFieldValue<T extends string | number>(field: FormField<T>): T {
    return field.value ?? field.defaultValue;
}
```
