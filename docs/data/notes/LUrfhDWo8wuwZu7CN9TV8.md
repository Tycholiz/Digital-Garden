
The different strategies of testing, whether it's [[unit testing|testing.method.unit]], [[integration testing|testing.method.integration]], [[E2E testing|testing.e2e]] etc., have a common concept of a **System Under Test (SUT)**. In unit and integration(?) tests, it is the actual module being tested. In E2E, it is the whole flow that is being tested.

Testing pyramid is a way to minimize quality risk per investment. Represents an ideal, easier to find issues earlier lower down

## Test Doubles
![[testing.test-double]]

## Making the test fail at first
The goal of doing TDD is to turn the red bar green. The key distinction here is that before we get to the green bar, we have to have the red bar.
"if your tests verify your code then what verifies your tests?". The answer is twofold:
- your tests verify the code
- your code verifies the tests

The purpose of making tests fail first is to improve confidence that we don't have false negatives. That is, we don't want to be in a position where tests are passing for a reason other than the fact that our code is working properly.
