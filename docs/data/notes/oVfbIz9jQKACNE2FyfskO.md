
The idea with TDD is that we write tests first for our module, then we write code that passes those tests.

We may also take a bit of an inverse approach with TDD, and write the code first. In this case, the idea is to write some production code – enough to pass a test or two – and then write those tests. Then, to determine how much code you need to pass a test or two, think of a couple of tests to pass, then write the code that would pass those tests. Then write the tests.
- In order to work in small chunks, you have to imagine the tests that you'll be writing; so that you can write code that is testable.
- The really effective part of TDD is the size of the cycle, not so much whether you write the test first. The reason we write the tests first is that it encourages us to keep the cycles really short.
- [source](https://blog.cleancoder.com/uncle-bob/2016/11/10/TDD-Doesnt-work.html)
