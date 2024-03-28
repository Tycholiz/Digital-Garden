Spies are [[stubs|testing.test-double.stub]] that also record some information based on how they were called.
- ex. One form of this might be an email service that records how many messages it sent.

For example, you’d use this when you wanted to be sure that the `login` method was called by your system.

In your test you’d inject it like a stub, but then at the end of the test you’d check the `loginWasCalled` variable to make sure the system actually called `login`.
- therefore, a Spy spies on the caller.

A Spy could be used to...
- count the number of invocations.
- see inside the workings of the algorithms you are testing.

The more you spy, the tighter you couple your tests to the implementation of your system, leading to fragile tests.
- spec: this is because we are spying on specific implementations of modules that are not within the SUT. Therefore, these units are subject to change without the SUT even being aware. If their implementation changes, we would start getting failing tests. In this case, the spy would have to be updated to use the new implementation.

```java
public class AcceptingAuthorizerSpy implements Authorizer {
  public boolean authorizeWasCalled = false;

  public Boolean login(String username, String password) {
	loginWasCalled = true;
	return true;
  }
}
```
