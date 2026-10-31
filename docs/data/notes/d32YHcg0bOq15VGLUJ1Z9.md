
A sanity test (or sanity check) is a basic test to quickly evaluate whether a claim (or the result of a calculation) can possibly be true.
- it offers "quick, broad, and shallow testing".

The point of a sanity test is to rule out certain classes of obviously false results, not to catch every possible error.

Sanity tests may sometimes be used interchangeably with [[smoke tests|testing.method.smoke]] insofar as both terms denote tests which determine whether it is possible and reasonable to continue testing further. However, there is a distinction:
- a sanity test determines whether the intended result of a code change works correctly
- a smoke test ensures that nothing else important was broken in the process.

Sanity tests are normally followed by more rigorous forms of testing.

### Example: Hello World
A "Hello, World!" program is often used as a sanity test for a development environment in a similar fashion. Rather than a complicated script running a set of unit tests, if this simple program fails to compile or execute, it proves that the supporting environment likely has a configuration problem that will prevent any code from compiling or executing

### Example: Multiplying by 9 divisibility rule
In arithmetic, we have the following divisibility rule for the number 9:
> To find out if a number is divisible by 9, sum the digits. If the result is divisible by 9, then the original number is too.

In practice,
> 2880: 2 + 8 + 8 + 0 = 18

Testing to make sure that this logic is fulfilled is a sanity test. It's not covering everything that could go wrong, but it's just giving us a reasonable baseline of expectation for our function.
