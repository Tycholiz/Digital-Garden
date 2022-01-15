
Lambda Extensions API can be used to integrate Lambda with their preferred tools for monitoring, observability, security, and governance.

Your extension can register for function and execution environment lifecycle events. In response to these events, you can start new processes, run logic, and control and participate in all phases of the Lambda lifecycle: initialization, invocation, and shutdown.

An extension runs as an independent process in the execution environment and can continue to run after the function invocation is fully processed.
- Because extensions run as processes, you can write them in a different language than the function, though a compiled language is recommended so the extension is a self-contained binary.
