
With integration tests, we are testing relationships between services
- A naive approach might be to get all of the dependent services up and running for the testing environment. But this is unnecessary, and creates a lot of potential failure points from services that our outside of our control.
	- Instead, we could narrow it down by writing a few service integration tests using mocks and stubs.

Integration tests use live deployed applications.

In integration testing, the rules are different from unit tests. Here, you should only test the implementation and functionality that you have the control to edit. Mocks and stubs could be used for this purpose.

In integration or [[E2E|testing.method.e2e]] testing, the first step to uncovering what tests to write is to understand what could go wrong.

Imagine we were making a banking app with the following 5 modules:
![](/assets/images/2021-12-06-12-39-40.png)

Even if we had all modules done except for the 5th, we could still test the integration. We would do this by stubbing the 5th service.

Integration tests are notoriously more difficult to write when using a [[Service-oriented architecture|general.arch.SOA]]

### Downsides
Integration tests give us confidence to release, but...
- introduce dependencies
- give slow feedback
- break easily
- require lots of maintenance