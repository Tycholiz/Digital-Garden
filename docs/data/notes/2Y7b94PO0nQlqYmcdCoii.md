
### Use an existing type as a base, but remove some keys and add some others
```ts
type GetCommandInput = Omit<__GetItemCommandInput, "Key"> & {
  Key: { [key: string]: NativeAttributeValue } | undefined
}
```