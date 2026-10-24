
Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.

Stubs are a kind of [[dummy|testing.test-double.dummy]]

Imagine you were testing a part of your system that required you to be logged in. You could simply log in to carry out actions in the test, but why test that functionality again, when we have (presumably) already tested it elsewhere?

Instead, let's just extend the regular `Authorizer` class to override `login` to just be a method that returns `true`.
- then, when we want to test the part of the system that expects an unauthorized user, we can just return `false` from the stub.

```java
@Test
public void newlyCreatedSystem_hasNoLoggedInUsers() {
	System system = new System(new AcceptingAuthorizerStub());
	system.authorizer.login('dude@dude.com', 'securePassword')
	assertThat(system.loginCount(), is(1));
}
```

* * *

Example: Your test class depends on a method `Calculate()` taking 5 minutes to complete. Rather than wait for 5 minutes you can replace its real implementation with a stub that returns hard-coded values; taking only a small fraction of the time.
