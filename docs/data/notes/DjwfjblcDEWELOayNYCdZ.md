
Factory functions allow us to create objects without having to specify the exact class of the object that will be created.
- This is accomplished by calling a function, rather than calling a constructor.

The idea is to define an interface for creating an object, but let subclasses decide which class to instantiate. The Factory method lets a class defer instantiation it uses to subclasses.

In the absence of factory functions, we may have some added problems, and an object's creation...
- may lead to a significant duplication of code,
- may require information not accessible to the composing object,
- may not provide a sufficient level of abstraction,
- may otherwise not be part of the composing object's concerns.

The factory method pattern relies on inheritance, as object creation is delegated to subclasses that implement the factory method to create objects.

The problem with constructors is that they look just like regular functions, but do not behave like regular functions at all.

The factory function pattern is similar to constructors, but instead of using new to create an object, factory functions simply set up and return the new object when you call the function.

With a factory
```js
const personFactory = (name, age) => {
  const sayHello = () => console.log('hello!');
  return { name, age, sayHello };
};

const jeff = personFactory('jeff', 27);

console.log(jeff.name); // 'jeff'

jeff.sayHello(); // calls the function and logs 'hello!'
```

With a constructor
```js
const Person = function(name, age) {
  this.sayHello = () => console.log('hello!');
  this.name = name;
  this.age = age;
};

const jeff = new Person('jeff', 27);
```

ES6 modules are actually very similar to factory functions. The main difference is how they’re created.
- The concepts are exactly the same as the factory function. The nuance is that instead of creating a factory that we can use over and over again to create multiple objects, the module pattern wraps the factory in an IIFE (Immediately Invoked Function Expression).
```js
const calculator = (() => {
  const add = (a, b) => a + b;
  const sub = (a, b) => a - b;
  const mul = (a, b) => a * b;
  const div = (a, b) => a / b;
  return {
    add,
    sub,
    mul,
    div,
  };
})();

calculator.add(3,5) // 8
calculator.sub(6,2) // 4
calculator.mul(14,5534) // 77476
```

above, the function inside the IIFE is a simple factory function, but we can just go ahead and assign the object to the variable calculator since we aren’t going to need to be making lots of calculators, we only need one.
