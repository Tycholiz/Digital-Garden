
JSON Schema allows us to validate our JSON documents (including through automated tests). We can create a `json` file for each domain object (ie. what we would find in each table of an [[SQL|sql]] db), and we specify things like...
- what properties this object has
- what types they are
- which ones are required
- what restraints they have (e.g. max length, regex match etc.)

The first key-value pair of the schema should be declaring which dialect of JSON Schema that our schema uses:
- this is called a *meta schema*, since it is a schema that describes another schema.
```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema"
}
```

## Code reuse
Like any other code, schemas are easier to maintain if they can be broken down into logical units that reference each other as necessary. 
- In order to reference a schema, we need a way to identify a schema. Schema documents are identified by non-relative URIs.

Schemas need an identifier (ie. `$id`) if we want to reference them from other schemas.
- schemas without an identifier are known as *anonymous schemas*.

### Methods
#### JSON Pointer:
![[json#json-pointer,1]]

#### $ref
The value of `$ref` is a URI-reference that is resolved against the schema’s Base URI (ie. the value of `$id`).

## Tools
### `json-schema-to-typescript`
We can use [this library](https://github.com/bcherny/json-schema-to-typescript) to generate types from the json schema.

### Ajv
We can use `ajv` to take in our json schema, and generate validation functions to make sure the data coming in adheres to the schema.

## Resources
- [Online JSON Schema Validator](https://www.jsonschemavalidator.net/)