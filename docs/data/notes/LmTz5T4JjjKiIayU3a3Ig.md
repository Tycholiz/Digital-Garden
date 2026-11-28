
# Object::String
The default conversion from an object to string is `[object Object]`
- when we see `[object Object]` somewhere, it would seem to imply that the object is getting coerced into a string somewhere along the line, thus resulting in us seeing `[object Object]` 
- check to see if you didn't properly stringify json
- output from `console.log` is captured at the application level, meaning that we won't get access to the logs if we are using multiple applications
	- ex. if we are using Azure Functions in our app, we are using multiple applications, since our codebase exists in a different application than our Azure Functions. 
