
### Reduce Function with double type coercion and overloading
```ts
function reduce <TElement, TReturn>(
    inputArr: TElement[], 
    reducerMethod: (accumulator: TReturn, currVal: TElement) => TReturn
): TReturn
function reduce <TElement, TReturn>(
    inputArr: TElement[], 
    reducerMethod: (accumulator: TReturn, currVal: TElement) => TReturn, 
    initialValue: TReturn
): TReturn
function reduce <TElement, TReturn>(
    inputArr: TElement[], 
    reducerMethod: (accumulator: TReturn, currVal: TElement) => TReturn, 
    initialValue?: TReturn
): TReturn {
    let acc: TReturn
    let currentIndex: number

    if (initialValue === undefined) {
        acc = inputArr[0] as any as TReturn
        currentIndex = 1
    } else {
        acc = initialValue
        currentIndex = 0
    }

    while (currentIndex < inputArr.length) {
        const currentVal = inputArr[currentIndex]
        acc = reducerMethod(acc, currentVal)
        currentIndex++
    }
    return acc
}
```

### Reduce function with unions
```ts
function reduce <TElement, TReturn>(
    inputArr: TElement[], 
    reducerMethod: (accumulator: TReturn | TElement, currVal: TElement) => TReturn
): TReturn
function reduce <TElement, TReturn>(
    inputArr: TElement[], 
    reducerMethod: (accumulator: TReturn | TElement, currVal: TElement) => TReturn, 
    initialValue: TReturn
): TReturn
function reduce <TElement, TReturn>(
    inputArr: TElement[], 
    reducerMethod: (accumulator: TReturn | TElement, currVal: TElement) => TReturn, 
    initialValue?: TReturn
): TReturn | TElement {
    let acc: TReturn | TElement
    let currentIndex: number

    if (initialValue === undefined) {
        acc = inputArr[0]
        currentIndex = 1
    } else {
        acc = initialValue
        currentIndex = 0
    }

    while (currentIndex < inputArr.length) {
        const currentVal = inputArr[currentIndex]
        acc = reducerMethod(acc, currentVal)
        currentIndex++
    }
    return acc
}
```
