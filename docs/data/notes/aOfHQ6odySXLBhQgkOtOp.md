
Serverless is actually "server-less" in the same way that Javascript is "memory-less".
- In JS, memory is still there, it's just abstracted away from us. Put another way, we don't have to think in terms of memory management, because it's all handled under the surface.

Serverless sometimes refers to the concept of "functions as a service", basically taking the microservices pattern and realizing "hey, because we're doing things this way, we basically can forget about the server"
- Somebody is still doing the heavy lifting (AWS, Google Cloud, etc.) but it's now an abstraction and you have limited ability to configure it, and this is fine if you're using it for the right use cases.

In the case of AWS, you use Lambda as the compute service to run the code and API Gateway to create API endpoints to point at the Lambda functions. Under the hood, once a Lambda is invoked through the event trigger which is the API Gateway endpoint, a Docker container is spun up to run the code you deployed into the cloud.
