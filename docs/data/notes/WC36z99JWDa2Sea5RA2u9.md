
`decimal` is an alias for `numeric`

there are 2 values inherent to a `numeric` value
1. Precision - total $ of significant digits in a number (ie. size of integer part plus size of decimal part)
2. Scale - size of decimal part. (therefore integers have scale of 0)
- these 2 values should be declared explicitly

example:
```sql
numeric(3,2)
```
means "3 digits total, with 2 occurring past the decimal"
ie. `1.00` and `0.23`, but not `1.235`
