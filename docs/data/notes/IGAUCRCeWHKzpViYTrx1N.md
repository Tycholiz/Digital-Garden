
# Overview
"function" is a reference to how functions work in math.
Ex. f(x) = 2x² + 5 can be plotted on a graph to make a parabola. In this sense, it can be thought of as a map. An x value of 5 maps to a return value of 55.
Though FP rarely deals with graphs like this, think of it as input values mapping to output
![](/assets/images/2021-03-09-09-35-55.png)

functional languages are closer to math than the more popular imperative languages.

Functions as [[first-class citizens|general.lang.feat.functions.first-class]] is a key component of Functional Programming, since using higher-order components to achieve composition is a standard practice (remember, `map`, `filter` and `reduce` are all HOCs)

Based on idea of Referential Transparency

### Referential transparency
The notion that a function could be replaced by its return value and it wouldn't impact the functionality of the program.
If satisfied, this is a clear sign that a function is pure.
as you're reading a program, once you've mentally computed what a pure function call's output is, you no longer need to think about what that exact function call is doing when you see it in code, especially if it appears multiple times.
That result becomes kinda like a mental const declaration, which as you're reading you can transparently swap in and not spend any more mental energy working out.


## Abstracting (Generalizing) Functions
Similar to how partial application and currying (see Chapter 3) allow a progression from generalized to specialized functions, we can abstract by pulling out the generality between two or more tasks. The general part is defined once, so as to avoid repetition. To perform each task's specialization, the general part is parameterized.

consider:
```js
function saveComment(txt) {
    if (txt != "") {
        comments[comments.length] = txt;
    }
}

function trackEvent(evt) {
    if (evt.name !== undefined) {
        events[evt.name] = evt;
    }
}
```
the repetition (generality) between the 2 functions is: storing a value in a data source. the uniqueness (specialty) of them is that one sticks the value on the end of an array, while the other sets the value as a property on an object.

abstracting:
```js
function storeData(store,location,value) {
    store[location] = value;
}

function saveComment(txt) {
    if (txt != "") {
        storeData( comments, comments.length, txt );
    }
}

function trackEvent(evt) {
    if (evt.name !== undefined) {
        storeData( events, evt.name, evt );
    }
}
```
