
API Gateway is a fully managed service that allows us to *create*, *publish*, *maintain*, *monitor*, and *secure* APIs at any scale

Allows us to create 2 types of API:
- RESTful APIs
- Web APIs, that enable real-time two-way communication applications

Handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including:
- traffic management
- CORS support
- authorization and access control
- throttling
- monitoring
- API version management

API Gateway’s REST API type allows users to setup HTTP proxy integrations, which can be used for forwarding incoming requests with different path patterns to different origin servers according to the API specifications
![](/assets/images/2021-12-07-12-18-22.png)

Supports containerized and serverless workloads, as well as web applications.

Pay for the API calls you receive and the amount of data transferred out

API Gateway is combined well with Lambda, whereby we make an endpoint and cause the endpoint to trigger the [[aws.svc.lambda]]
