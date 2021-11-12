
when you are refactoring some piece of code, all you need to ask is "did the signature of what I just refactored change?" put another way, were the inputs and outputs of the changed code modified, so that code that depends on it would no longer work properly? 
- Any time you are changing code, always be mindful of the code that depends on it. Think of how that depending code consumes the code that you are refactoring
