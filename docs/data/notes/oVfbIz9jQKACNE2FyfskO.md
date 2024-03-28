
The objective of TDD is to test behaviours in the system.
- this is accomplished by writing tests first that describe behaviour of the software, then writing code that passes those tests.

While TDD is accomplished with [[testing.method.unit]], the *unit* does not necessarily correpond to a class/module. Instead, the system under test can be thought of as an API that implements some behaviour. As a result, a unit may encompass multiple classes, so long as they all achieve some higher level behaviour of the system.

## Behaviour nature of TDD
TDD could be thought of as *behavioural testing*, if that term wasn't already appropriated by [[development-process.agile.BDD]]. The idea is that we view isolation as modules of behaviour, rather than as individual classes. We define our units as modules that don't cross a port (such as making a network call, or accessing the filesystem). In other words, we test at the level of our *units*, which perform some self-isolated action that we can test for. Therefore, a unit may involve more than one class. The implementation details are immaterial. What matters is that we can test the inputs and outputs of this unit (the surface area). What this means is that we can structure our unit tests in such a way where we are testing for behaviour. Then, once all the tests are written, we can start to implement the code. Subsequent refactorings of the code shouldn't involve changes to the tests, since tests don't worry about implementation details. The implementation could be one big function, or it could be several.
- here, isolation is about not crossing ports, rather than sticking to a single class.

Typically, the trigger to following a TDD cycle is the necessity of a new method on a class. People think "I need to add a method that does this and that, so I will first write a test to cover that implementation, then write the method to make all tests pass". This is the wrong way to go about it.
- instead, the trigger should be that the system has a new requirement. Therefore, "I want the software to do this new thing, so I will start by writing tests that test that this system does that thing".
- this shows how testing should be declarative, not imperative.
  - we don't test that `foo()` gets called 2 times when `bar()` is called. This is a flaky test, since if our implementation changes, it's likely the test will change too.
- Thought of another way, we want to test the API of our software, not the implementation details.

When we think of individual classes as our units, we tend to mock more heavily. This leads to overspecified tests, whereby we know all the implementation details of our classes via our mocks and how they are called.
- an overspecified test is a test that makes too many assumptions about how the system under test is implemented, rather than focusing on the system's surface (API). These assumptions lead to tests that break when implementation details change.

Because of the focus on software behaviour, TDD forces us to think in terms of what the consumer of that API would find useful. What results are tests that match the way that consumers actually use the code.

### Red, Green, Refactor
Commonly thought to mean *"write a test, make sure it fails. Write some code to pass the test. Tidy up a bit"*, but actually means:
1. Write a test that represents the behaviour that is needed from the system. It compiles, but the test fails.
  - this represents a requirement for the program
  - we want to see our test fail because there are no tests for tests.
2. Write some minimal code to make the test pass. This is as quick-n-dirty as it needs to be, and involves no patterns, no designs, and no structure. Just stick code in a method that might not even belong in that class. As long as it gets the job done, it's good
  - we do this because once we've made the test pass, we've understood how to implement the requirements. 
    - at this point we can move fast. We can feel more free to go to Stackoverflow and copy some code. 
  - another reason we do this is because we are focusing on one thing at a time: get the behaviour of the code right, then engineer it well. It is far more difficult to do both at the same time, and what often results is either an overengineered solution, or analysis paralysis.
3. Add design: Extract methods, implement a design pattern, create additional classes etc. Clean up the quick-n-dirty
  - refactoring is a process of making safe moves that let us change the design of the code without changing the behaviour. Therefore, no new tests are written at this point.
  - refactoring is about changing implementation details, not about how that interface has changed. If we think of this in terms of [[OOP|paradigm.oop]] it can be thought of as *"refactoring doesn't happen in your public properties, only your private ones"*. This of course is not absolute, and is more of a guideline. The idea is that we don't want to change our public API, because others depend on it. Instead, our refactoring should focus on how something has been implemented.

When we approach testing this way, we move away from creating [[test doubles|testing.test-double]] for each class of our classes to depend on, and instead start to create test classes only to replace dependencies at the port (e.g. at the level of network call, file access etc.) 

If we isolate testing at the level of classes (as is more typically done), what happens is that we find ourselves having to modify the tests whenever the implementations themselves get refactored. Most of the time the culprit is [[mocks|testing.test-double.mock]] that need to be changed.
- modifying tests becomes more problematic down the road when we forget how our tests work (since tests are typically more imperative than the implementations, they can be more difficult to understand)

* * *

We may also take a bit of an inverse approach with TDD, and write the code first. In this case, the idea is to write some production code - enough to pass a test or two - and then write those tests. Then, to determine how much code you need to pass a test or two, think of a couple of tests to pass, then write the code that would pass those tests. Then write the tests.
- In order to work in small chunks, you have to imagine the tests that you'll be writing; so that you can write code that is testable.
- The core insight of TDD is the size of the cycle, not so much whether or not you write the test first. The reason we write the tests first is that it encourages us to keep the cycles really short, but we would get pretty much the same benefit if we took a code-first approach, but in similar size cycles.
- [source](https://blog.cleancoder.com/uncle-bob/2016/11/10/TDD-Doesnt-work.html)

## E Resources
- https://www.stevefenton.co.uk/2013/05/my-unit-testing-epiphany/
- https://www.youtube.com/watch?v=EZ05e7EMOLM