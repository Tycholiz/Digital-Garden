
## `Omit<Type, Keys>`
`Omit` will construct a new type by taking the `Type` and removing the `Keys` (if multiple, represented as a union of string literals)

### Example:
Make a new type `QueryCommandInput`, which consists of the `__QueryCommandInput` without the `KeyConditions` or `QueryFilter` keys.

`type QueryCommandInput = Omit<__QueryCommandInput, "KeyConditions" | "QueryFilter">`
