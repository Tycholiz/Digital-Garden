
OpenAPI is a specification for describing REST API formats. Swagger provides tools for implementing that specification. These tools can be used at different stages of the API lifecycle.

An OpenAPI file (json or yml) allows you to describe your entire API, including:
- Available endpoints (`/users`) and operations on each endpoint (`GET /users`, `POST /users`)
- Operation parameters Input and output for each operation
- Authentication methods
- Contact information, license, terms of use and other information.

The value of OpenAPI is that it allows APIs to describe their own structure. This allows us to:
- Design-first users: use Swagger Codegen to generate a server stub for your API. The only thing left is to implement the server logic – and your API is ready to go live!
- Use Swagger Codegen to generate client libraries for your API in over 40 languages.
- Use Swagger UI to generate interactive API documentation that lets your users try out the API calls directly in the browser.
- Use the spec to connect API-related tools to your API. For example, import the spec to SoapUI to create automated tests for your API.

Definition example:
```
openapi: 3.0.0
info:
  title: Sample API
  description: Optional multiline or single-line description in [CommonMark](http://commonmark.org/help/) or HTML.
  version: 0.1.9
servers:
  - url: http://api.example.com/v1
    description: Optional server description, e.g. Main (production) server
  - url: http://staging-api.example.com
    description: Optional server description, e.g. Internal staging server for testing
paths:
  /users:
    get:
      summary: Returns a list of users.
      description: Optional extended description in CommonMark or HTML.
      responses:
        '200':    # status code
          description: A JSON array of user names
          content:
            application/json:
              schema: 
                type: array
                items: 
                  type: string
```
