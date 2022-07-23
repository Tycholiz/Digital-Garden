
### JSON Pointer
A JSON Pointer describes a slash-separated path to traverse the keys in the objects in the document. Therefore, `/properties/street_address` means:

1. find the value of the key `properties`
2. within that object, find the value of the key `street_address`

The URI `https://example.com/schemas/address#/properties/street_address` identifies the highlighted subschema in the following schema.

```json
{
  "$id": "https://example.com/schemas/address",
  "type": "object",
  "properties": {
    "street_address":
      { "type": "string" }
  }
}
```