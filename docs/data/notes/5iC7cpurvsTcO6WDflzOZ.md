
# How it works
Node.js is a JavaScript runtime environment that processes incoming requests in a loop, called the event loop.
Node runs as a single process. Other languages may handle "concurrency" by having multiple threads handle tasks. However, Node achieves this by having async. When an async action happens, the javascript code will not get blocked, and will continue to execute until the response has come. This makes node a very event-based language (hence its usefulness in the web word, which is fundamentally made up of requests). 
- concurrency here is used liberally, since Node does not run its code concurrently at all. Alas, this is how Node solves the problem, which may be solved using multiple threads (concurrency) at a time. 

Node is built on top of v8, the engine that converts javascript to native machine code. These core javascript features form the basis of what Node is. Because Javascript is missing native features that server languages typically have, there comes a need of having to fulfill those tasks. Web server functions (such as I/O, networking, streams etc.) are fulfilled by having isolated modules that each have a single responsibility. Node.js is made up of many of these modules, all built on top of the base language of Javascript and its underlying v8 engine.
- ques:does this mean that all the language features of javascript are also available in node? Is there a global object? what about all of the javascript features that are considered useless, like exec? 

Node.js connects the ease of a scripting language (JavaScript) with the power of Unix network programming

Node.js uses an event loop for scalability, rather than processes or threads

## V8
When it was first introduced, javascript was a simple scripting language, and processed commands in real time. This had performance implications, and made it very slow. This was the inspiration for V8, which did just in time compiling, making it a much faster and more capable language.

With Node, the idea was "what if we took the V8 engine, and instead of using it in the context of the web, we allowed it to run terminal applications"
## Resources
### UE Resources
[Tutorial: creating a native node module with C++](https://medium.com/@marcinbaraniecki/extending-node-js-with-native-c-modules-63294a91ce4)
[Tutorial: creating streams in node](https://github.com/substack/stream-handbook)
