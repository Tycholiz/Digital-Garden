
Often, in order to test a module in isolation we need other modules that help the main module perform its action. In cases such as these, we need *test doubles* to stand in for the actual modules.

The word “mock” is often used in an informal way to refer to Test Doubles.
- Semantically speaking, a "real mock" refers to the kind described below. In colloquial parlance, it refers indiscriminately to all 5.

There are 5 types of Test Double:
- [[Dummy|testing.test-double.dummy]] objects
- [[Fake|testing.test-double.fake]] objects
- [[Stubs|testing.test-double.stub]]
- [[Spies|testing.test-double.spy]]
- [[Mocks|testing.test-double.mock]] are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.

All Test Doubles can and usually do state verification, but only mocks insist upon behavior verification.

Test Doubles need to make the SUT believe it's talking with its real collaborators.

[[Stubs|testing.test-double.stub]] and [[Spies|testing.test-double.spy]] can probably cover most use-cases for test doubles.

*"stubs and spies are very easy to write. The IDE makes it trivial. You just point at the interface and tell the IDE to implement it. Voila! It gives you a dummy. Then you just make a simple modification and turn it into a stub or a spy. So you seldom need an actual mocking tool."*

## Authorizer example
Imagine we had a `System` class, which accepts an `Authorizer` as a parameter. That is, when we instantiate a new `system` object, it is associated with some `authorizer`. Different Test Doubles are used depending on what we want to do in the test.
```java
public class AcceptingAuthorizerStub implements Authorizer {
  public Boolean login(String username, String password) {
		return true;
	}
}

public class System {
	public System(Authorizer authorizer) {
		this.authorizer = authorizer;
	}

	public int loginCount() {
		//returns number of logged in users.
	}
}
```
While testing the `System` class, if we:
- don't even care about the `authorizer` object (since we are not calling any of its methods within the test), we would use a dummy.
- wanted to simply return `true` for the `authorizer.login()` method, we would use a stub.
	- here, we just want to know that we are logged in so we can test the parts of the `System` that require us to be logged in.
- want to assert that the `login()` method was called by the `authorizer` object, we would use a spy.

* * *

### Mocking vs Stubbing
A stub is a simple fake object. It just makes sure test runs smoothly.
A mock can be thought of as a smarter stub. You verify your test passes through it.

Mocks are used to assert and should never return data, stubs are used to return data and should never assert.

A Mock is interaction-based; Stubs are state-based.
- This means you don't expect from Mock to return some value, but to assume that specific order of method calls are made.

Stubs test how your SUT handles receiving messages, mocks test how your SUT sends messages.

a stub returns answers to questions. A mock also returns answers to questions, but it also verifies that the question was asked

there may be several stubs in one test, but generally there is only one mock.

A Stub is written with predetermined behavior.
- The idea is you would have a class that implements the dependency (abstract class or interface most likely) you are faking for testing purposes and the methods would just be stubbed out with set responses. They would not do anything fancy and you would have already written the stubbed code for it outside of your test.

Stubs don't care that correct methods have been invoked in mock.

A mock is set up with the expectations.
- Mocks in a way are determined at runtime since the code that sets the expectations has to run before they do anything.

Tests written with mocks usually follow an initialize -> set expectations -> exercise -> verify pattern to testing. While the pre-written stub would follow an initialize -> exercise -> verify.

Both mocks and stubs testing give an answer for the question: What is the result?
Testing with mocks are also interested in: How the result has been achieved?

# E Resources
[A conversation about "Mocking"](https://blog.cleancoder.com/uncle-bob/2014/05/14/TheLittleMocker.html)
