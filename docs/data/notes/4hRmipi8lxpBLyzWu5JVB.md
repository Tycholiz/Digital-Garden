
## What is it?
OpenAPI is a specification for describing REST API formats. 
- Tools like Swagger provide tools for implementing that specification. These tools can be used at different stages of the API lifecycle.
  - note: Swagger is the prior name of OpenAPI.

OpenAPI 3.0 uses an extended subset of [[json-schema]] to describe data formats.
- this means that some keywords are supported and some are not. See the [breakdown](https://swagger.io/docs/specification/data-models/keywords/)

An OpenAPI file (json or yml) allows you to describe your entire API, including:
- Available endpoints (`/users`) and operations on each endpoint (`GET /users`, `POST /users`)
- Operation parameters Input and output for each operation
- Authentication methods
- Contact information, license, terms of use and other information.

## Why use it?
The value of OpenAPI is that it allows APIs to describe their own structure. This allows us to:
- Design-first users: use Swagger Codegen to generate a server stub for your API. The only thing left is to implement the server logic – and your API is ready to go live!
- Use Swagger Codegen to generate client libraries for your API in over 40 languages.
- Use Swagger UI to generate interactive API documentation that lets your users try out the API calls directly in the browser.
- Use the spec to connect API-related tools to your API. For example, import the spec to SoapUI to create automated tests for your API.

## Example
```yml
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

## Implementation 
### `$ref`
- [main docs](https://swagger.io/docs/specification/using-ref/)
`$ref` can be used to reference models from other models.

For example, imagine we had a model for `invoice_items.json`, and we wanted to reference an `invoice.json` model found in the same `schema/` directory.
```json
{
  // ...
  "properties": {
    "id": { "type": "string" },
    "invoice": { "$ref": "invoice.json" }
    }
  },
  // ...
}
```

`$ref` can also be used to reference a specific value within the same JSON document (JSON pointer notation).
- ex. `#/components/schemas/user`


* * *

# Tools
### Ajv
We can use `ajv` to take in our json schema, and generate validation functions to make sure the data coming in adheres to the schema.

### Redoc
Redoc takes the OpenAPI definitions (yml file) and generates HTML that can be viewed in the browser.
- [docs](https://github.com/Redocly/redoc)
