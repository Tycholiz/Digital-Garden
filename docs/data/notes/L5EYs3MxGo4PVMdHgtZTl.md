
With integration tests, we are testing relationships between services
- A naive approach might be to get all of the dependent services up and running for the testing environment. But this is unnecessary, and creates a lot of potential failure points from services that our outside of our control.
- Instead, we could narrow it down by writing a few service integration tests using mocks and stubs.

In integration testing, the rules are different from unit tests. Here, you should only test the implementation and functionality that you have the control to edit. Mocks and stubs could be used for this purpose.