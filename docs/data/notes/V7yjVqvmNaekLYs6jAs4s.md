
## Inclusive/Exclusive Bounds
- `[]` is used to represent inclusive values
- `()` is used to represent exclusive values
- ex. `SELECT '[3,7)'::int4range` is a range that includes 3, but discludes 7

## Omitting Upper/Lower Bounds
We can omit upper bounds with `[7,]`
We can omit lower bounder with `[,7]`
