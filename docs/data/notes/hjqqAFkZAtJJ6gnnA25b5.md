
## body-parser
- when we don't use body-parser, we get the raw request in the request body. In this format, the `body` and `headers` keys are not on the root object of the request parameter (ie. they are nested). This means that we must individually manipulate all the fields
