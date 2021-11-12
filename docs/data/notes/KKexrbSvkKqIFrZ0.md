

### Setup
spec: this is a [[harness|testing.harness]]
Before running tests, it is often useful to prepare the grounds so that the tests can do their work.
- We can draw parallels this with the world of manual testing, where a tester would need to seed their database before being able to effectively perform their tests.

ex. if we are running tests against the database, then we might want to get some preliminary data in place. We might also want to establish a `pgPool` for us to query with.

### Teardown
After we are done running the tests, we might want to end the `pgPool` connection, or maybe we want to delete all test users from the db

## Snapshot testing
snapshots can capture any serializable value and should be used anytime the goal is testing whether the output is correct.

Your tests should be deterministic. Running the same tests multiple times on a component that has not changed should produce the same results every time.
- if you have a `Clock` component that uses `Date.now()`, the snapshot generated each time the test is run will be different. To fix this, mock the `Date.now()` method.

In some scenarios, snapshot testing can potentially remove the need for unit testing for a particular set of functionalities (e.g. React components), but they can work together as well.

* * *

#### Common pitfall: not separating interface from implementation#
Because some classes may have references to other classes, testing a class can frequently spill over into testing another class.

A common example of this is with classes that depend on a database: in order to test the class, the tester often writes code that interacts with the database. This is a mistake, because a unit test should not usually go outside of its own class boundary, and should especially not cross process/network boundaries because doing so can introduce unacceptable performance problems to the unit test-suite. Crossing such unit boundaries turns unit tests into integration tests, so when test cases fail, it is much less clear which component is causing the failure.

Instead, the software developer should create an abstract interface around the database queries, and then implement that interface with their own mock object. By abstracting this necessary attachment from the code (temporarily reducing the effective net coupling), the independent unit can be more thoroughly tested than may have been previously possible. This results in a higher quality unit that is also more maintainable.