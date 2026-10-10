
Fakes actually have working implementations, but usually take some shortcut which makes them not suitable for production (an in memory database is a good example).

Fakes are not written very often, and can probably be avoided outright.

a Fake has business behavior. You can drive a fake to behave in different ways by giving it different data.
- it can therefore be thought of as a simulator.

Fakes aren’t stubs because fakes have real business behavior; stubs do not.

We can say that a Mock is a kind of spy, a spy is a kind of stub, and a stub is a kind of dummy. But a fake isn’t a kind of any of them. It’s a completely different kind of test double.

They can so complicated that they need unit tests of their own. At the extremes the fake becomes the real system.
