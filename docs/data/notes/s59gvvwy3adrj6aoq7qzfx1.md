
Instance variables always get a default value.
- integers, chars - 0
- floating points - 0.0
- booleans - false
- references - null

*Local variables* exist within a method.
- they do not get default values

## "Global" variables
While there isn’t a concept of 'global' variables and methods in a Java, marking a method as `public` and `static` makes it behave much like a 'global'. 
- Any code, in any class of your application, can access a `public static` method.  
  - this is exactly how the `Math.pi` property and `Math.random()` methods are declared.
- And if you mark a variable as `public`, `static`, and `final`, you have essentially made a globally-available constant. 
- these static (global-like) things are the exception rather than the rule in Java. They represent a very special case, where you don’t have multiple instances.

## Primitive vs Object Reference Variables
Variables can either be a *primitive* or a *reference*

### Primitive
Primitive variables hold fundamental values (think: simple bit patterns) including integers, booleans, and floating point numbers. They are the most basic building blocks.
- int, long, float, double, boolean, char, byte, short

#### Numbers
(all are signed)

- Byte - 8 bits
- Short - 16 bits
- Int - 32 bits
- Long - 64 bits

- Float - 32 bits with decimal
- Double - 64 bits with decimal


### Reference
Reference variables hold a reference to the object.
- that is, the object itself does not go into the variable; the variable only holds a pointer to it.

They are based on a class made up of primitives
- String, Scanner, Random, Die, int[], String[]

anal: think of a *reference* as a variable that holds a remote control. The remote control knows how to control the object that it refers to.
- Using the dot operator (`.`) on a reference variable is like pressing a button on the remote control to access a method or instance variable.

![](/assets/images/2023-01-18-19-59-12.png)

Anything that is not a primitive is an object.

the `==` operator is used only to compare the bits in two variables
- if we are using the `==` operator to compare 2 objects, what we are really doing is comparing the object references. If both variables reference the same object, then we will get `true`
- therefore:
```java
int a = 3;
byte b = 3;
if (a == b) /* true */
```

When an object is created, the following steps happen:
1. The JVM allocates space for a reference variable, and gives it a name and a type
2. The JVM allocates space for the new object on the heap
3. The reference variable is linked to the object

Because an object reference is simply a pointer, they are all the same size

#### Null Pointer Exceptions
Reference variables can be set to `null`, which means "I am referencing nothing"
- If you try to dereference one of these variables (either explicitly by you or through Java automatically) you get a `NullPointerException`

The `NullPointerException` (NPE) typically occurs when you declare a variable but did not create an object and assign it to the variable before trying to use the contents of the variable.

```java
public void doSomething(SomeObject obj) {
   // Do something to obj, assumes obj is not null
   obj.myMethod();
}

// if the method gets called with a null value, we will get a NPE
doSomething(null);
```

## Garbage Collection
Each time an object is created, it goes into an area of memory known as The Heap.
- it’s not just any old memory heap; the Java heap is actually called the Garbage-Collectible Heap. 
- When you create an object, Java allocates memory space on the heap according to how much that particular object needs. 
  - ex. An object with, say, 15 instance variables, will probably need more space than an object with only two instance variables. 
- When the JVM can see that an object is no longer referenced by a variable, that object becomes eligible for garbage collection