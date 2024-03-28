
Expressions are used to denote the attributes that you want to read from an item.

Expressions are strings that check for the validity of a particular statement.

With expressions, we can use comparator operators (`=`, `<`, `>`), as well as built-in functions like `attribute_exists()`, `contains()`, `size()` and `begins_with()`
- ex. use `attribute_not_exists(DateShipped)` to ensure we are not updating an item that has already shipped.

You also use expressions when writing an item to indicate any conditions that must be met, and to indicate how the attributes are to be updated.

By default, all items are returned with GET operations (like `GetItem`, `Query`, or `Scan`).
- to get only some items back, we use a *projection expression*, which is a string that identifies the attributes that you want.

Expressions are used in a few different areas:
- *condition expression* - update an item when certain conditions are true
- *projection expression* - specify a subset of attributes you want to receive when reading Items
- *update expression* - update a particular attribute in an existing Item.
- *key condition expression* - query a table with a composite primary key to limit the items selected.
- *filter expression* - filter the results of queries to make responses more efficient.

Sometimes we cannot accurately represent our desired statement due to DynamoDB syntax limitations or when it's easier to use variable substitution to create the statement rather than building a string.
- we can use expression attribute names and expression attribute values to get around these limitations.
- these allow you to define expression variables outside of the expression string itself, then use replacement syntax to use these variables in your expression.

### Expression attribute names
An expression attribute name is a placeholder that you use in an DynamoDB expression
- begins with `#`

There are times where we want to write an expression for a particular attribute, but can't properly represent that attribute name due to DynamoDB limitations. These limitations are:
- the attribute is a [reserved word](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ReservedWords.html)
- attribute name contains a `.`
- attribute name begins with a number

To get around this, we pass in a map where the key is the placeholder to use in our expression and the value is the attribute name we want to expand to.
```js
{
  "#ts": "timestamp"
}
```

Then, we can use `#a` when we want to refer to our `Age` attribute.

### Expression attribute values
These are substitutes for the actual values that you want to compare— values that you might not know until runtime.
- begins with `:`

Similar to expression attribute names except that they are used for the values used to compare to values of attributes on an Item, rather than the name of the attribute.
- unlike expression attribute names, we must also specify the type:
```js
{":agelimit": {"N": 21} }
```

For example, we could have a `scan` operation that returns us all items from the `ProductCatalog` table where the color is black and the size is less than $500:

```sh
aws dynamodb scan \
    --table-name ProductCatalog \
    --filter-expression "contains(Color, :c) and Price <= :p" \
    --expression-attribute-values { \
        ":c": { "S": "Black" }, \
        ":p": { "N": "500" } \
      } \
    }
```



