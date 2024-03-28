
When the `throw` statement is called, execution of the current function will stop and control will be passed to the first catch block in the call stack.
- If no catch block exists among caller functions, the program will terminate.

executing `throw` generates an exception that penetrates through the call stack.

In practice, the exception you throw should always be an Error object or an instance of an Error subclass, such as RangeError
- this is because code that catches the error may expect certain properties, such as [message](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/message), to be present on the caught value. For example, web APIs typically throw DOMException instances, which inherit from Error.prototype.

### `try...catch`
If any statement within the try block (or in a function called from within the try block) throws an exception, control immediately shifts to the catch block. 
- If no exception is thrown in the try block, the catch block is skipped. 

#### `finally`
- The finally block executes after the try and catch blocks execute but before the statements following the `try...catch` statement.
- You can use the finally block to make your script fail gracefully when an exception occurs. 
    - ex. you may need to release a resource that your script has tied up (e.g. close db connection, close a file)
- code within the `finally` will *always* execute. Even if the `catch` block returns a value, a return in the `finally` block will overwrite it.
    - therefore, if there is a return value in the `finally` block, that will always be the return value of the `try..catch..finally` block.
