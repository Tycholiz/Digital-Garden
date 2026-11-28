
When you are refactoring some piece of code, all you need to ask is "did the signature of what I just refactored change?" put another way, were the inputs and outputs of the changed code modified, so that code that depends on it would no longer work properly? 
- Any time you are changing code, always be mindful of the code that depends on it. Think of how that depending code consumes the code that you are refactoring

Before doing any refactoring, it's a good idea to draw out the components that you are refactoring, as well as any existing dependencies that use the code you are refactoring. This is important to know, because if you modify any API, and dependent code is going to also need to be modified.

## Resources
- [Guide to code smells and associated refactorings](https://refactoring.guru/refactoring)
- [Refactoring](https://refactoring.com/)