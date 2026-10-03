
We pass a dummy object to something when we don't care how the dummy is used

Dummy objects are passed around but never actually used. Usually they are just used to fill parameter lists.
- We use dummies when we don't actually care about the parameters that are passed to the SUT.

### Example: System Class
imagine we are testing a `System` object. When we create an instance of `System`, we must specify who the authorizer is. However, for the particular test we are writing, maybe we don't even care about the authorizer, and that parameter is immaterial to the test. A case such as this would be a test where there is not even a user logging in. In this case we would pass the argument, but it would never even get used. In this case we would just use a dummy authorizer, and move on with our day:
```java
public class System {
	public System(Authorizer authorizer) {
		this.authorizer = authorizer;
	}

	public int loginCount() {
		//returns number of logged in users.
	}
}

public class DummyAuthorizer implements Authorizer {
  public Boolean authorize(String username, String password) {
		return null;
	}
}

@Test
public void newlyCreatedSystem_hasNoLoggedInUsers() {
	System system = new System(new DummyAuthorizer());
	assertThat(system.loginCount(), is(0));
}
```
